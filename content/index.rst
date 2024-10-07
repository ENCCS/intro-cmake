Introduction to CMake
=====================


CMake is a language-agnostic, cross-platform build tool and is nowadays the *de facto* standard, with large projects using it to reliably build, test, and deploy their codebases.

**CMake is not a build system itself, but it generates another system's build files.**

In this workshop, you will learn

- Write a CMake build system for C/C++ and Fortran projects producing libraries and/or executables.
- Ensure your build system will work on different platforms.
- Detect and use external dependencies in your project.
- (optional) Run tests for your code with `CTest`.
- (optional) Safely and effectively build mixed-language projects (Python+C/C++, Python+Fortran, Fortran+C/C++)



.. prereq::

   Before attending this workshop, please make sure that you have access to a computer with a compiler for your favorite programming language and a recent version of CMake.
   
   If you have access to a supercomputer (e.g. a `NAISS system <https://www.naiss.se/>`_) with a compute allocation you can use that during the workshop. Any practical questions on how to use a particular HPC resource should be directed to the appropriate support desk.
   
   You can also use your own computer for this workshop, provided that it has the necessary tools installed.

   - If you do not already have these installed, we recommend that you set up an isolated software environment using ``conda``.
   - For Windows computers we recommend to use the **Windows Subsystem for Linux (WSL)**. Detailed instructions can be found on the :doc:`setup` page.


.. toctree::
   :hidden:
   :maxdepth: 1

   setup


.. toctree::
   :hidden:
   :maxdepth: 1
   :caption: The lesson

   hello-cmake
   cmake-syntax
   targets
   probing
   dependencies
   tips-and-tricks
   additional-topics


.. csv-table::
   :widths: auto
   :delim: ;

   30 min ; :doc:`hello-cmake`
   30 min ; :doc:`cmake-syntax`
   40 min ; :doc:`targets`
   30 min ; :doc:`probing`
   20 min ; :doc:`dependencies` 
   10 min ; :doc:`tips-and-tricks`
   10 min ; :doc:`additional-topics`


.. toctree::
   :maxdepth: 1
   :caption: Reference

   quick-reference
   instructor-guide



.. _learner-personas:



Who is the course for?
----------------------

This course is for students, researchers, engineers, and programmers that have heard of `CMake <https://cmake.org/>`_ and want to learn how to use it effectively with projects they are working on.
This course assumes no previous experience with `CMake <https://cmake.org/>`_. You will have to be familiar with the tools commonly used to build software in your compiled language of choice (C/C++ or Fortran).

Specifically, this lesson assumes that participants have some prior experience with or knowledge of the following topics (but no expertise is required):

- Compiling and linking executables and libraries.
- Differences between shared and static libraries.
- Automated testing.



About this course
-----------------

This lesson material is originally developed by the `EuroCC National Competence Center Sweden (ENCCS) <https://enccs.se/>`_ and taught in the `CMake Workshop <https://enccs.github.io/cmake-workshop/>`_. Each lesson episode has clearly defined learning objectives and includes multiple exercises along with solutions, and is therefore also useful for self-learning.

This material `Introduction to CMake <https://enccs.github.io/intro-cmake/>`_ was adapted from the lesson materials for `CMake Workshop (ENCCS) <https://enccs.github.io/cmake-workshop/>`_ and `CMake Workshop (CodeRefinery) <https://coderefinery.github.io/cmake-workshop/>`_, and will be use for the `Build Systems Course and Hackathon <https://enccs.se/events/build-systems-course-and-hackathon-2024/>`_.

This lesson material is licensed under `CC-BY-4.0 <https://creativecommons.org/licenses/by/4.0/>`_ and can be reused in any form (with appropriate credit) in other courses and workshops. Instructors who wish to teach this lesson can refer to the :doc:`guide` for practical advice.



Outreach
--------

There are many free online resources regarding CMake:

- The `CMake official documentation <https://cmake.org/cmake/help/latest/command/cmake_minimum_required.html>`_.
- The `CMake tutorial <https://cmake.org/cmake/help/v3.19/guide/tutorial/index.html#guide:CMake%20Tutorial>`_.
- The `HEP Software Foundation <https://hsf-training.github.io/hsf-training-cmake-webpage/>`_ training course.
- The `Building portable code with CMake <https://coderefinery.github.io/cmake/>`_ from the `CodeRefinery <https://coderefinery.org/>`_.
- The `CMake Workshop <https://coderefinery.github.io/cmake-workshop/>`_ from the `CodeRefinery <https://coderefinery.org/>`_.


You can also consult the following books:

- **Professional CMake: A Practical Guide** by Craig Scott.
- **CMake Cookbook** by Radovan Bast and Roberto Di Remigio. The accompanying repository is on `GitHub <https://github.com/dev-cafe/cmake-cookbook>`_



Credits
-------

The lesson file structure and browsing layout is inspired by and derived from the `work <https://github.com/coderefinery/sphinx-lesson>`_ by `CodeRefinery <https://coderefinery.org/>`_ licensed under the `MIT license <http://opensource.org/licenses/mit-license.html>`_. We have copied and adapted most of their license text.



Instructional Material
^^^^^^^^^^^^^^^^^^^^^^

All ENCCS instructional material is made available under the `Creative Commons Attribution license (CC-BY-4.0) <https://creativecommons.org/licenses/by/4.0/>`_. The following is a human-readable summary of (and not a substitute for) the `full legal text of the CC-BY-4.0 license <https://creativecommons.org/licenses/by/4.0/legalcode>`_. You are free:

- to **share** - copy and redistribute the material in any medium or format;
- to **adapt** - remix, transform, and build upon the material for any purpose, even commercially.


The licensor cannot revoke these freedoms as long as you follow these license terms:

- **Attribution** - You must give appropriate credit (mentioning that your work is derived from work that is Copyright (c) ENCCS and, where practical, linking to `<https://enccs.se>`_), provide a `link to the license <https://creativecommons.org/licenses/by/4.0/>`_, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.
- **No additional restrictions** - You may not apply legal terms or technological measures that legally restrict others from doing anything the license permits. With the understanding that:

	- You do not have to comply with the license for elements of the material in the public domain or where your use is permitted by an applicable exception or limitation.
	- No warranties are given. The license may not give you all of the permissions necessary for your intended use. For example, other rights such as publicity, privacy, or moral rights may limit how you use the material.



Software
^^^^^^^^

The code samples and exercises in this lesson were adapted from the GitHub repository for the `CMake Cookbook <https://github.com/dev-cafe/cmake-cookbook>`_.

Except where otherwise noted, the example programs and other software provided by ENCCS are made available under the `OSI <http://opensource.org/>`_-approved `MIT license <http://opensource.org/licenses/mit-license.html>`_.

