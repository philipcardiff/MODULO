---
title: 'MODULO: a Python toolbox for reduced order modeling'
tags:
  - Python
  - reduced order modeling (ROM)
  - structures identification
  - dft
  - pod
  - mpod
  - dmd
  - spod
authors:
  - name: L. Schena
    affiliation: 1,3
  - name: D. Ninni
    affiliation: 2
  - name: M. A. Mendez
    affiliation: 1
affiliations:
 - name: Department of Environmental and Applied Fluid Mechanics, the von Karman Institute (VKI) for Fluid Mechanics
   index: 1
 - name: Department of Aerospace Engineering, Politecnico di Bari
   index: 2
 - name: Department of Mechanical Engineering, Vrije Universiteit Brussels (VUB)
   index: 3
date: 09 December 2022
output: bookdown::html_document2
bibliography: paper.bib
---

# Summary

Dimensionality reduction is an essential tool in processing large datasets, enabling data compression, pattern recognition and reduced order modelling. Many linear tools for dimensionality reduction have been developed in fluid mechanics, where they have been formulated to identify coherent structures and build reduced-order models of turbulent flows [@berkooz_proper_1993]. 
This work proposes a major upgrade of the software package MODULO (MODal mULtiscale pOd,[@ninni_modulo_2020]), which was designed to perform Multiscale Proper Orthogonal Decomposition (mPOD)[@mendez_balabane_buchlin_2019]. In addition to implementing the classic Fourier Transform (DFT) and Proper Orthogonal Decomposition (POD), MODULO now also allows for computing Dynamic Mode Decomposition (DMD) [@schmid_2010] as well as the Spectral POD by [@sieber_paschereit_oberleithner_2016] and the Spectral POD by [@Towne_2018].
All algorithms are wrapped in a ‘SciKit’-like Python API, which enables implementing all decompositions within a line of code. Documentation, exercises, and video tutorials are also provided to offer a primer on data drive modal analysis.


# Statement of need

As extensively illustrated in recent reviews (e.g., [@mendez_2023], [@Taira2020], all modal decompositions can be seen as special kinds of matrix factorizations. The matrix being factorized, herein denoted as $D$, collects (many) snapshots (samples) of a large dimensional variable.
The factorization aims at providing a basis for the column and the row spaces of the matrix, with the goal of identifying the most essential patterns (modes) according to a certain criterion. In the common arrangement encountered in fluid dynamics, the basis for the column space is a set of ‘spatial structures’ while the basis for the row space is a set of `temporal structures’. These are paired by a scalar, called amplitude or initial value, which defines their relative importance. 
In the POD, known as Principal Component Analysis in other fields, the modes are identified as the ones that have the largest amplitudes while remaining orthonormal both in space and time.  In the classic DFT, as implemented in MODULO, modes are defined to evolve as orthonormal complex exponential in time. This implies that the associated frequencies are integer multiples of a fundamental tone. The DMD generalizes the DFT by releasing the constraint of orthogonality and considering complex frequencies, i.e., modes that can potentially vanish or decay. 
Both the constraint of energy optimality and harmonic modes can lead to poor performances in terms of convergence and feature detection. This motivated the development of hybrid methods such as the the Spectral POD by [@Towne_2018], Spectral POD by [@sieber_paschereit_oberleithner_2016], Multiscale Proper Orthogonal Decomposition (mPOD)[@mendez_balabane_buchlin_2019]. The first can be seen as an optimally averaged DMD while the second consists in bringing POD and DFT with the use of a filtering operation. Both the SPODs assume statistically stationary data and are mostly meant for identifying harmonic (or quasi-harmonic) modes. The mPOD on the other hand, consists in bridging POD with Multi-resolution Analysis (MRA), to provide modes that are optimal within a pre-scribed frequency band. The resulting mode are thus spectrally less narrow than those obtained in the SPODs, allowing also for their localization in time.
While many excellent Python APIs are available to POD, DMD and both SPODs (see for example [@pyDMD]) ([@pyPOD], [@Mengaldo2021], [@SpyOD]), MODULO is currently the only opensource tool for computing multiscale POD while still allowing to compute all the others.



# New features
sDMD, SPOD

This release of `MODULO` includes the Spectral Proper Orthogonal Decomposition (SPOD) of Towne et al. [@Towne_2018] and the one proposed by Sieber et al. [@sieber_paschereit_oberleithner_2016]. Even though these go with the same name, their algorithms are noticeably different. The Towne's methodology splits the dataset $D(\mathbf{x}, t)$ in $n_b$ equal portions, scaled in time. Then, the DFT must computed on each block, originating a tensor of size $(n_s \times n_f \times n_b)$, where $n_s$ are the degree of freedom of the dataset - i.e. spatial points, $n_f$ depends on the frequency bins of the DFT and finally $n_b$ are the different blocks assembled before, depending on the time horizon of the measurement. Finally, the classical POD is performed on each frequency bin, originating $n_f \times rank(D)$ modes. 
On the other hand, the Spectral approach of Sieber et al. consists in filtering the correlation matrix $\mathbf{K} \in \mathbb{R}^{n_t \times n_t}$, computed as in a normal POD decomposition. In particular, a pass-bass filter is applied on its diagonals which enforces the POD to be closer to a DFT. In fact, for a stationary process the $\mathbf{K}$ matrix would become a Toeplitz circulant matrix, which eigenvalues are indeed the DFT modes. After being filtered, then the diagonalization proceeds as for the POD. 

# Conclusion
\approx 700 characters: what is the goal of modulo?

# Acknowledgments
See if needed to acknowledge something/someone (@miguel)

# References