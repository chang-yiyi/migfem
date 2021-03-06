This is the README file for MIGFEM--the Isogeometric Analysis (IGA) Matlab code.
The code supports one, two and three dimensional linear elasticity problems.
Extended IGA for hole, inclusion and crack modelling is also implemented. 
Structural mechanics problems including Euler beam and Kirchoff plates/shells
(rotation-free formulations), Reissner-Mindlin plates are implemented as well.

The features of the code include:

- Global h-refinement using knot insertion is provided for one and two
dimensional meshes. 

- p- and k-refinement via Geopdes library are also supported.

- Extended IGA which is a Partition of Unity enrichment IGA for 2D traction-free
cracks is also implemented. Level sets are used to detect enriched nodes.
However, enrichment functions are defined in terms of standard geometry i.e.
not in terms of level sets. Holes and inclusions are also implemented.

- An ad hoc implementation for 3D cracks is also provided. In this case,
level sets are used both for enrichment detection and enrichment function evaluation.

- Visualization of displacements, stresses are done in Paraview by exporting the
results to a VTU file. Mesh for visualization purpose is Q4 in 2D and B8 in
3D where B8 denotes tri-linear brick elements.

- Inhomogeneous Dirichlet boundary conditions are treated with the penalty
method, Lagrange multiplier method and the least square method. 

- Numerical integration: 

   (o) elements cut by cracks: sub-triangulation 
   (o) elements cut by circular holes/inclusions: adaptive sub-cells (used in Finite Cell Method)

- Fast assembly of FE stiffness using the triple sparse matrix format
(see files of which names end with FastAssembly in folder iga)

- Tsplines based on Bezier extraction operators. See folder bezier_extraction.
- Bsplines/NURBS basis functions are implemented using MEX files for better performances.

- Beams and plates/shells without rotation dofs. See folder structural_mechanics.
- Modal analysis. See folder structural-mechanics/.
- 2D/3D large displacement/rotation continuum elements (Total Lagrange).
- Disretisation and model coupling with Arlequin and Nitsche method (development version).
- Material Point Method 2D (development version)

Written by:

 Vinh Phu Nguyen, 
 Delft University of Technology, The Netherlands
 Cardiff University, Wales, UK
 nvinhphu@gmail.com and
 
 which uses MEX files implementation of Bsplines written by
 Robert Simpson 
 Cardiff University, Wales, UK

Please cite our paper when using this package:

V.P. Nguyen,  C. Anitescu, S. Bordas, T. Rabczuk
"Isogeometric analysis: an overview and computer implementation aspects".
Mathematics and Computers in Simulation, 117, 89-116, 2015.

Before using the M files, you should first compile some C codes implemented as MEX files.
Open Matlab and in the command line, type the following commands (make sure your PC has C compiler
such as gcc and you did mex -setup in Matlab)

compile

Actually compile.m contains commands to compile MEX files. For details on MEX files, we refer to
http://www.mathworks.com/support/tech-notes/1600/1605.html. Our MEX files were compiled on Mac OS Lion and Ubuntu
with gcc 4.2.1 and gcc version 4.7.2 20121109 (Red Hat 4.7.2-8) (GCC) (Tested /w Fedora 18).
We have not tested with other compilers. Please consult the aforementioned website for details.

REMARKS: 

The displacement vectors are organized as
U=[ux_1 uy_1 ux_2 uy_2 ...]          in 2D
U=[ux_1 uy_1 uz_1 ux_2 uy_2 uz_2...] in 3D
For the Mindlin plate elements, the unknown vector is structured as
U=[ux_1 theta_x1 theta_y1 ux_2 theta_x2 theta_y2 ...]

The package is structured into sub-folders which are described in what follows.

I. Main script files: one file for one problem. The names of the files are already
self explainary. These files can be found in folder "iga".

*******************************************************************
1. igaLShaped.m
2. igaPlateCircularHole.m
3. igaPlateTension.m
4. igaTimoshenkoBeam.m
5. igaCurvedBeam.m
6. igaPlateCircularInclusion.m
*******************************************************************

Do not run the file 'iga2d.m'. This file is the first version of
the code and is not compatible with later improvement.

and files start with "xiga" for linear elastic fracture mechanics problems. 
These files can be found in folder "xiga".

1. xigaEdgeCrack.m
2. xigaEdgeShearCrack.m
3. xigaTwoEdgeCracks.m
4. xigaCenterCrack.m
5. xigaInfiniteCrackModeI.m
6. xigaInfiniteCrackModeII.m
7. xigaInfiniteCrackModeILM.m
8. xigaInfiniteCrackModeILeastSquare.m
9. xigaEdgeCrack3d.m
10.xigaInfiniteCrack3d.m

and some simple 3D elasticity problems

1. iga3dBeam.m
2. igaThickCylinder.m
3. igaPinchedCylinder.m

We refer to folder bezier-extraction for some 2D elasticity problems but now
implemented using the Bezier extraction operators.

Structural mechanics examples can be found in folder structural-mechanics.

II. Data files: control points, knots, orders. These files can be found in
folder "data".


*******************************************************************
5. plateHoleData.m
6. timoshenkoBeamData.m
7. timoshenkoBeamC1Data.m
8. LShapedData.m
9. LShapedC1Data.m
10. plateC1Data.m
11. plateC2Data.m
*******************************************************************

III. Functions or procedures used in main scripts. They are located in different sub-folders depending on their functionalities.

Most of them have comments and quite self explanatory. I therefore
do not describe them here. For post-processing, the following routines are
used:

  1. plotStress1: compute displacements and stresses at nodes of a
visualization mesh (Q4 mesh) and export the data to a VTU file.
  2. plotStressXIGA: does just the same thing for XIGA problems.
  3. crackedMesh      : visualize cracked mesh as truly cracked domain (Q4 FEM
only)
  4. crackedMeshNURBS : the same but for high-order NURBS

IV. C_files: contains C implementation of some NURBS algorithms.

V. FEM problem files: start with "fem". These files can be found in folder
"fem".

These files are FEM codes to solve the same problems solved with IGA.
Used as reference solution to check IGA implementation.
Usually the FEM code reads GMSH mesh files (files with extension *.msh).
The *.msh files are created from the GMSH geometry files *.geo.

VI. Some stand alone scripts used to generate figures in the paper.
These files can be found in folder "examples".


1. circle.m:            a full circle by quadratic NURBS curve
2. curves1dWeights.m:   effects of weights on NURBS curves
3. deCasteljauExample.m
4. C_files/NURBSplotter.m: plot B-spline basis  functions 
5. discontinuity1d.m: an example showing the insertion of a knot p+1 times to create a discontinuity 
6. hRefinementExample.m: illustrate h-refinement for 1D B-splines

VI. Output files (folder "results")

   Output files include postscript files (*.EPS) and VTU files (*.vtu).
   VTU files can be processed by Paraview to plot displacements, stresses
   contours.

VII. The NURBS toolbox: in folder "nurbs-geopdes". Some routines in this toolbox are used for p- and k-refinement.

VIII. Finite Cell Method: simple implementation of the finite cell method with B-spline basis functions. The corresponding files are in folder "finite-cell-method".

IX. Multi-patch IGA code: compatible multi-patch IGA code is resided in folder "@patch2D".
