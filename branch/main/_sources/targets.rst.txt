.. _targets:


Target-based build systems with CMake
=====================================


.. questions::

   - How can we handle complex projects with CMake?
   - What exactly are **targets** in the CMake domain-specific language (DSL)?

.. objectives::

   - Learn that the **basic elements** in CMake are not variables, but targets.
   - Learn about properties of targets and how to use them.
   - Learn how to use **visibility levels** to express dependencies between targets.
   - Learn how to handle multiple targets in one project.
   - Learn how to work with projects spanning multiple folders.



Real-world projects require more than compiling a few source files into executables and/or libraries. In most cases, you will come to projects comprising hundreds of source files sprawling in a complex source tree.


With the advent of CMake 3.0, also known as **Modern CMake**, there has been a significant shift that the CMake domain-specific language (DSL) is structured, which can help you keep the complexity of the build system in check. Rather than relying on **variables** to convey information in a project, all what you need in modern CMake is **targets** and **properties**.



Targets
-------


A target is the basic element in the CMake DSL, which can be declared by either ``add_executable`` or ``add_library``. Any target has a collection of **properties**, which define:

* *how* the build artifact should be produced,
* *how* it should be used by other targets in the project that depend on it.


.. figure:: img/target.svg
   :align: center

    Compile options, definitions, include directories, source files, link libraries, and link options are properties of targets. These properties can be read with ``get_target_property`` and modified with ``set_target_properties``.


In CMake, the five most used commands used to handle targets are:

- ``target_sources``, specifying which source files to use when compiling a target.
- ``target_compile_options``, specifying which compiler flags to use.
- ``target_compile_definitions``, specifying which compiler definitions to use.
- ``target_include_directories``, specifying which directories will contain header (for C/C++) and module (for Fortran) files.
- ``target_link_libraries``, specifying which libraries to link into the current target.


There are additional commands in the ``target_*`` family:

.. code-block:: bash

   $ cmake --help-command-list | grep "^target_"
   
   target_compile_definitions
   target_compile_features
   target_compile_options
   target_include_directories
   target_link_directories
   target_link_libraries
   target_link_options
   target_precompile_headers
   target_sources



Visibility levels
-----------------


Why it is robust to use targets and properties than using variables? Given a target ``tgtX``, we can invoke one command in the ``target_*`` family as follows.

.. code-block:: cmake

   target_link_libraries(tgtX
     PRIVATE tgt1
     INTERFACE tgt2
     PUBLIC tgt3
     )


.. figure:: img/target_inheritance.svg
   :align: center

   Properties on targets have varied **visibility levels**, which determine how CMake should propagate them between interdependent targets.


.. typealong:: Understanding visibility levels

   Visibility levels ``PRIVATE``, ``PUBLIC``, or ``INTERFACE`` are very powerful and herein we will briefly demonstrate their difference.

   A complete source code is available in the ``content/code/04_visibility-levels/`` folder.
	
   In this code example, we want to compile a C++ library and an executable:

      - The library code is in the ``account`` subfolder. It consists of one source (``account.cpp``) and one header file (``account.hpp``).
      - The header file and the shared library are needed for the ``bank.cpp`` to produce the ``bank`` executable.
      - We also want to use the ``-ffast-math`` compiler flag and propagate it throughout the project.

   Thus code structure is arranged in the following format:


   1. The ``account`` target declares the ``account.cpp`` source file as ``PRIVATE`` since it is only needed to produce the shared library.

      .. code-block:: cmake

         target_sources(account
           PRIVATE
             account.cpp
           )

   2. The ``-ffast-math`` is instead ``PUBLIC`` as since it needs to be propagated to all targets consuming ``account``.

      .. code-block:: cmake

         target_compile_options(account
           PUBLIC
             "-ffast-math"
           )

   3. The ``account`` folder is an include directory with ``INTERFACE``
      visibility because only targets consuming ``account`` need to know where
      ``account.hpp`` is located.

      .. code-block:: cmake

         target_include_directories(account
           INTERFACE
             ${CMAKE_CURRENT_SOURCE_DIR}
           )


.. callout:: Rule of thumb for visibility settings

   When working out which visibility settings to use for the properties of your
   targets you can refer to the following table:

      ==============  ================ ============
        Who needs?             Others
      --------------  -----------------------------
       Target            **YES**           **NO**
      ==============  ================ ============
         **YES**         ``PUBLIC``     ``PRIVATE``
          **NO**       ``INTERFACE``        N/A
      ==============  ================ ============


An additional code example to demonstrate the difference of the visibility levels ``PRIVATE``, ``PUBLIC``, or ``INTERFACE`` is available in the `CodeRefinery CMake Workshop <https://coderefinery.github.io/cmake-workshop/targets/#visibility-levels>`_ lesson materials.



Properties
----------


CMake lets you set properties at many different levels of visibility across the project:

- **Global scope**. These are equivalent to variables set in the root ``CMakeLists.txt``. Their use is, however, more powerful as they can be set from *any* leaf ``CMakeLists.txt``.
- **Directory scope**. These are equivalent to variables set in a given leaf ``CMakeLists.txt``.
- **Target**. These are the properties set on targets that we discussed above.
- **Test**.
- **Source files**. For example, compiler flags.
- **Cache entries**.
- **Installed files**.


For a complete list of properties known to CMake:

.. code-block:: bash

   $ cmake --help-properties | less


You can get the current value of any property with ``get_property`` and set the value of any property with ``set_property``.



Multiple folders
----------------


In the code example about the visibility levels, we have two ``CMakeLists.txt`` files, one in the ``account`` subfolder and one in the main folder. This enables the code maintenance being easier if we split the CMake configuration into multiple ``CMakeLists.txt`` with the help of ``add_subdirectory``. Our goal is to have multiple ``CMakeLists.txt`` files as close as possible to the source files.

.. code-block:: text

   project/
   ├── CMakeLists.txt           <--- Root
   ├── external
   │   ├── CMakeLists.txt       <--- Leaf at level 1
   └── src
       ├── CMakeLists.txt       <--- Leaf at level 1
       ├── evolution
       │   ├── CMakeLists.txt   <--- Leaf at level 2
       ├── initial
       │   ├── CMakeLists.txt   <--- Leaf at level 2
       ├── io
       │   ├── CMakeLists.txt   <--- Leaf at level 2
       └── parser
           └── CMakeLists.txt   <--- Leaf at level 2


Each folder in a multi-folder project will contain a ``CMakeLists.txt``: a source tree with one **root** and many **leaves**.

- The root ``CMakeLists.txt`` will contain the invocation of the ``project`` command: variables and targets declared in the root have effectively global scope.
- The ``PROJECT_SOURCE_DIR`` will point to the folder containing the root ``CMakeLists.txt``.
- In order to move between the root and a leaf or between leaves, you will use the ``add_subdirectory`` command.



Typically, you only need to pass the first argument: the folder within the build tree will be automatically computed by CMake. We can declare targets at any level, not necessarily the root: a target is visible at the level at which it is declared and all higher levels.



.. exercise:: Exercise 05: Cellular automata

   Let's work with a project spanning multiple folders. We will implement a relatively simple code to compute and print to screen elementary `cellular automata <https://en.wikipedia.org/wiki/Cellular_automaton#Elementary_cellular_automata>`_. We separate the sources into ``src`` and ``external`` to simulate a nested project which reuses an external project.

   Your goal is to:

   - 1. Build the main executable at ``content/code/05_automata/cxx/`` for C++ and ``content/code/05_automata/fortran/`` for Fortran.
   - 2. Where are the obtained executables located in the build tree? Remember that CMake generates a build tree mirroring the source tree.
   - 3. The executable will accept 3 arguments: the length, number of steps, and
     automaton rule. You can run it with:

     .. code-block:: bash

        $ automata 40 5 30

     The output will be:

     .. code-block:: text

        length: 40
        number of steps: 5
        rule: 30
                            *
                           ***
                          **  *
                         ** ****
                        **  *   *
                       ** **** ***

   .. typealong:: The internal dependency tree

      You can visualize the dependencies between targets in the project with Graphviz (make sure that you have installed the Graphviz package):

      .. code-block:: bash

         $ cd build
         $ cmake --graphviz=project.dot ..
         $ dot -T svg project.dot -o project.svg

      .. figure:: img/graphviz-multiple-folder-project.svg
         :align: center

         The dependencies between targets in the cellular automata project.



.. keypoints::

   - Using **targets**, you can achieve granular control over how artifacts are built and how their dependencies are handled.
   - Compiler flags, definitions, source files, include folders, link libraries, and linker options are **properties** of a target.
   - Avoid using variables to express dependencies between targets: use visibility levels ``PRIVATE``, ``INTERFACE``, ``PUBLIC`` and let CMake figure out the details.
   - To keep the complexity of the build system at a minimum, each folder in a multi-folder project should have its own CMake script.

