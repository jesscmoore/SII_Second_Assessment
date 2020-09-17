
SII Second Stage Assessment Question 3
--------------------------------------

Introduction
------------
In this document, I will be displaying a part of the code I wrote during GSOC 2019 for open source software MDAnalysis. I have also added the Problem Statement of the project at the end if more context is needed.

Specifically, I have added the keyword 'unwrap' to multiple functions to get the correct result when Periodic Boundary Conditions are used.

Installation
------------
MDAnalysis Installation: https://www.mdanalysis.org/MDAnalysisTutorial/installation.html

Code
--------------------------
Pull Requests: [#2275](https://github.com/MDAnalysis/mdanalysis/pull/2275), [#2279](https://github.com/MDAnalysis/mdanalysis/pull/2279), [#2286](https://github.com/MDAnalysis/mdanalysis/pull/2286), [#2288](https://github.com/MDAnalysis/mdanalysis/pull/2288), [#2290](https://github.com/MDAnalysis/mdanalysis/pull/2290),

Usage
-----

The keyword unwrap allows the user to unwrap the molecules before calculating to property.
I have added unwrap keyword to various methods (Each PR corresponds to a different method).
An example of the usage of unwrap is shown below.

```python
from MDAnalysisTests.core.util import UnWrapUniverse

u = UnWrapUniverse(is_triclinic=False)
group = u.atoms[31:39]

# get center with unwrap
center1 = group.center(weights=None, pbc=False, compound='segments', unwrap=True)
# center1 is [[10.1000002, 7.5, 7.]]

# get center without unwrap
center2 = group.center(weights=None, pbc=False, compound='segments', unwrap=False)
# center1=2 is [[5.1, 7.5, 7. ]]

```





The Problem Statement (Why is unwrap needed?)
---------------------------------------------
Periodic Boundary Conditions(PBC) are extensively used in 
Molecular Dynamics to approximate a larger system using a small lattice.
For example, see Fig below

![Residue](https://raw.githubusercontent.com/NinadBhat/NinadBhat.github.io/master/images/gsoc_report/basic_image.png) 

if PBC are applied to this and atoms are wrapped
into simulation cell the atoms would appear as below

![Residue wrapped](https://raw.githubusercontent.com/NinadBhat/NinadBhat.github.io/master/images/gsoc_report/wrapped_basic_images.png)

Often atoms positions are represented as above in data files.

**Note the above atom positions can be obtained from the following code.**
```python
from MDAnalysisTests.datafiles import (
    TRZ_psf, TRZ,
)

universe = mda.Universe(TRZ_psf, TRZ)
group = universe.residues[2]
```

Whether a molecule splits across the periodic boundary or not,
effects calculations of various properties.


![Projection](https://raw.githubusercontent.com/NinadBhat/NinadBhat.github.io/master/images/gsoc_report/no_select.png)

The figure above shows four periodic images projected on the X-Y plane.


If the atoms are wrapped about the PBC, the atoms in red shown below are selected,
![select_wrap](https://raw.githubusercontent.com/NinadBhat/NinadBhat.github.io/master/images/gsoc_report/select_wrap.png)[^2]

Now if we calculate the Moment of Interia(MoI), for the wrapped atoms we get the following value
```
[[2177.3019906,  -237.02156344, -554.46003421]
 [-237.02156344, 2548.66172155, -381.71081487]
 [-554.46003421, -381.71081487, 2557.54815044]]
```
Which is wrong because it includes atoms 3 different periodic images.

However, if we unwrap the atoms, the atoms shown below are selected.

![select_unwrap](https://raw.githubusercontent.com/NinadBhat/NinadBhat.github.io/master/images/gsoc_report/select_unwrap.png)

Now considering the selected positions for calculations of MoI we get the following values,
```python
[[2820.12756093,  297.26274974, -143.17261246]
 [ 297.26274974, 2390.56824468, 1030.83862229]
 [-143.17261246, 1030.83862229, 1860.72757184]]
```
These values are correct as they consider an entire molecule for the calculation of MOI.


Hence we need to unwrap atoms before calculating the properties.
