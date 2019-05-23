****************
PCL_ Basics
****************
The source code of PCL_ library can be found on github: ::

    https://github.com/PointCloudLibrary/pcl/

In the source directory you can find ``tools``, ``examples`` and ``test``. In the ``doc`` directory you can find the ``tutorials`` source code as well as the explanation in ``reStructuredText``.
The tutorials_ are also accessible on the PCL_ website under documentation_.

when installed the tools are available as command line. For example you can run ``pcl_viewer`` ::

  pcl_viewer table_scene_mug_stereo_textured.pcd

The command will open a window with point cloud data. The pcd file can be found in the tutorials source code.
A lot of point cloud files can be found also in the ``test`` directory.

Third party file may have the ``ply`` file format, The command line ``pcl_ply2pcd`` can be used to get pcd files. ::

  // ASCII format
  pcl_ply2pcd 0 input.ply output.pcd
  // binary format
  pcl_ply2pcd 1 input.ply output.pcd

First example
================

Read a point file, pcd, and wiew the content.

.. literalinclude:: ../../../code/pcl/first_example/first_example.cpp
  :language: c++

.. literalinclude:: ../../../code/pcl/first_example/CMakeLists.txt
  :language: cmake

To build and run the example: ::

  mkdir build
  cd build
  cmake ..
  make

  ./first_example

Data structures
================
The main data structure in PCL is the template class ``pcl::PointCloud<pcl::PointTemplate>``. The template types could be:

  - ``pcl::PointXYZ``: 3D XYZ
  - ``pcl::PointXYZI``: 3D XYZ plus intensity of the point
  - ``pcl::PointXYZRGB``: 3D XYZ plus rgb colors
  - ``pcl::Normal``: it represents the surface normal at a given point and a measure of its curvature
  - ``pcl::PointNormal``: 3D XYZ coordinates and the surface normal

Refer to the documentation for a complete list of supported point types. Note that you can define you own data types.

Create a ``PointCloud`` object with ``PointXYZ`` point type::

  pcl::PointCloud<pcl::PointXYZ> cloud;

Create a ``Boost`` pointer ::

  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZ>);


Algorithms design patterns
============================

The following code read point cloud data, apply a filter then save the filtered data to a file.

.. literalinclude:: ../../../code/pcl/statistical_removal/statistical_removal.cpp
  :language: c++

Algorithms in ``PCL`` are almost all templated and follow the same structure.
First an algorithm object is created, parameters are set using setter accessors and then the result is accessed.

In this example, ``a pcl::StatisticalOutlierRemoval`` filter is created. The number of neighbors to analyze for each point is set to 50, and the standard deviation multiplier to 1.
What this means is that all points who have a distance larger than 1 standard deviation of the mean distance to the query point will be marked as outliers and removed. The output is computed and stored in cloud_filtered.

.. code-block:: c++

  // Create the filtering object
  pcl::StatisticalOutlierRemoval<pcl::PointXYZ> sor;
  sor.setInputCloud (cloud);
  sor.setMeanK (50);
  sor.setStddevMulThresh (1.0);
  sor.filter (*cloud_filtered);

Changing the filter parameters we can get the outliers:

.. code-block:: c++

  sor.setNegative (true);
  sor.filter (*cloud_filtered);

Refer to the official tutorial, **Writing a new PCL class**, to understand better how algorithm are designed.

PCD file format
==================
Different file formats are supported by ``PCL``. The ``PCD`` that is ``Point Cloud Data`` can be found in binary format or in ASCII format.
The following snippet represent a pcd file where are saved 5 point clouds of type x,y,z. ::

  # .PCD v.5 - Point Cloud Data file format
  FIELDS x y z
  SIZE 4 4 4
  TYPE F F F
  WIDTH 5
  HEIGHT 1
  POINTS 5
  DATA ascii
  0.35222 -0.15188 -0.1064
  -0.39741 -0.47311 0.2926
  -0.7319 0.6671 0.4413
  -0.73477 0.85458 -0.036173
  -0.4607 -0.27747 -0.91676

PCL and ROS
=============

PCL and Qt
============




.. _PCL: http://www.pointclouds.org
.. _documentation: http://www.pointclouds.org/documentation/
.. _tutorials : http://www.pointclouds.org/documentation/tutorials/index.php
