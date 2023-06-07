# Period-matrices
Magma code for Period matrices for Abelian varieties
* Grupo800-980.mgm. Contains the symplectic representation for G(800,980) on genus 101 with signature (0;8,8,2). Obtained with polyNoZ.m in https://github.com/rojas-ani/magma-routines. Outputs: 
  - a, b: Symplectic representation of the generators of G(800,980) of order 8 (corresponding to elements fixing points). The element of order 2 is ab.
  - G: The image of G(800,980) in Sp(202,Z)
  - H: The symplectic image of the unique abelian subgroup of G(800,980) of order 100.
* ActionGSubvariety.mgm. Code for Theorem 4.1. Contains several functions:
  - RestrictedRepresentation(L,b): L is expected to be a set of generators of G given as symplectic matrices in some basis B of an ambient ppav A, and b the coordinate matrix of a symplectic basis of a G-subvariety with respect to the same simplectic basis B. The output is a list of the same generators as in L but now represented on the basis b.
  - InducedPolarization(e): e is an idempotent represented in a symplectic basis for a ppav A. The output are bN, Jind: the coordinate matrix of a symplectic basis of Im(a) and the induced polarization on it.
  - PH(H): H is a subgroup of G (symplectic). The output is (the representation in the symplectic basis of) the idempotent associated to H, $p_H=1/|H| \sum_{h\in H} h$.
  - ActionGSubJacobian(L,H): L set of symplectic generators of G, H a symplectic subgroup of G. If the image of $p_H$ is G-invariant, it computes the action of G restricted to it. The output is a record with ph the idempotent $p_H$, bN the coordinates of a symplectic basis for the lattice of $A_H:=Im(p_H)$, Jind the induced polarization on $A_H$, LH the action of G restricted to $A_H$. Otherwise gives *error*.
  - ActionGSubvariety(L,e): Same as above for $A^e=Im(e)$, provided it is a G-subvariety. For instance, when e is a central idempotent (given as a symplectic matrix).
  - IsotypicalFactorAll(G): G as a symplectic group. It computes all the central idempotents, the dimensions of the isotypical factors and the primitive ones (in GAD). It also identify which ones are positive dimensional, among other information.
  - IsotypicalFactor(G,tb,rep): Input: G (symplectic), tb (character table of G), rep (the number of the representation whose isotypical component you want to study). The output is a record with the dimension of the isotipical component corresponding to that complex irreducible representation, the dimension of the primitive factor (in GAD), the central idempotent, the Schur index.
  - Subvariety(p,Av); Input a endomorphism p represented in the symplectic basis of the abient variety A, a record Av containing the information as in the output of IsotypicalFactorAll.

**Example** Section 7: A genus 101 curve with completely decomposable Jacobian. The group G(800,980) is acting on a curve X of genus 101 with signature (0;8,8,2). The corresponding Jacobian variety JX decomposes has *Group algebra decomposition* (GAD): $S\times E_1\times E_2^2\times E_3^8\times \dots\times E_{14}^8$, where $E_j$ is an alliptic curve, and $S$ an abelian surface. Let H be the unique abelian subgroup of order 100. $S$ is isogenous to $Im(p_H)$. We compute the induced polarization (JH'Jind), the restricted action (JH'LH), and the Riemann matrix of $S$ as follows:  
> load "Grupo800-980.mgm";  
> load "ActionGSubvariety.mgm";  
> JH:=ActionGSubJacobian([a,b],H);  
> Attach("polyDZ.m");  
> z:=MoebiusInvariantDZ([10,10], JH`LH);  

* Grupo98-28.mgm. Contains the symplectic representation for G(98,28) on genus 11 with signature (0;24,4,2). Obtained with polyDZ.m in https://github.com/rojas-ani/magma-routines. Outputs: 
  - a9828, b9828: Symplectic representation of the generators of G(98,28) of order 24 and 4 respectively (corresponding to elements fixing points). The element of order 2 is their product a9828 $\cdot$ b9828.
  - G9828: The image of G(98,28) in Sp(22,Z)

**Example** Section 8: A genus 11 curve with completely decomposable Jacobian of CM type. The group G(98,28) is acting on X of genus 11 with signature (0;24,4,2). The GAD of the corresponding Jacobian variety JX is $E_1\times E_2^2\times E_3^2\times E_4^2\times S^2$, where $E_j$ is an elliptic curve and $S$ an abelian surface. The goal is to determine the $\tau$ (hence the lattice) for each $E_j$ and the period matrix for $S$. Firstly, we have computed the symplectic representation for the action of $G$ on $X$ using polyDZ.m, so here we begin by uploading its output. Then, we proceed as shown below.  
> load "Grupo98-28.mgm";  
> IdentifyGroup(G9828);  
> load "ActionGSubvariety.mgm";  
> T:=IsotypicalFactorAll(G9828);  
> T`NFactGen, T'dimA, T'dimB;  
> central_idemp:=T'Idemp;  
> polSubAV:=[InducedPolarizationType(x): x in central_idemp];  
> Attach("polyDZ.m");   
> SubAV:=[ActionGSubvariety([a9828,b9828],x): x in central_idemp];  
> z:=[MoebiusInvariantDZ(polSubva[j],SubAV[j]'LH): j in [1..\#polSubva]];    

Up to this point we have, among other data, the induced polarization (polSubVA) on the isotypical factors $A^e=Im(e)$ for each $e$ central idempotent in $\mathbb{Q}[G]$, and the Riemann matrix (z) for each $A^e$.
To decompose further, consider that the first isotypical component is simple (it is an elliptic curve), and the primitive factors for the second to the fourth isotypical components are isogenous to the image of $p_H$ for some subgroup $H$. First, we study the decomposition of $Ind_H^G(\chi_0)$ for every subgroup $H$ of $G$  we want. From this, we identify which subgroups are useful to find primitive factors for GAD. We do:  
> HH:=Subgroups(G9828: IndexLimit:=95);                                   
> rHH:=[x`subgroup: x in HH];                           
> desc:=InducedRepByTrivial(G9828,rHH,T'Tb);  
> T'NrRepr;  
> desc[21];   

The variable *desc* is a matrix containing the multiplicity of each complex irreducible representation (indexed as in T'Tb) of $G$ in $Ind_H^G(\chi_0)$ for $H$ in the given list rHH. Rows indexed by subgroups and columns by representations. The index in T'Tb corresponding to each isotypical component is storaged in T'NrRepr. Here, the second component corresponds to T'Tb[13], and its primitive factor ($E_2$) is isogenous to $Im(p_H)$ for $H=rHH[21]$. So in order to apply our Theorem 3.1, we proceed as follows:
> A2:=SubAV[2];   
> pH21:=PH(rHH[21]);  
> SS:=Subvariety(pH21,A2);  

In SS'bN, we have the coordinates of a symplectic basis for the lattice of $A_2^{p_H}=Im(p_H)\subset A_2$ with $A_2=Im(e_2)$. In SS'Jtype, the type of the induced polarization in $A_2^{p_H}$ by the polarization in $A_2$ (which is induced by the polarization in the ambien variety $A$).

Recall that for $A_2$, we also have its induced polarization and its Riemann matrix.  We now move to Sagemath and work as shown below, to find the Riemann matrix W of the primitive factor $A_2^{p_H}$ in the isotypical component $A_2$. We use the elements we got from our magma routines:

> K=CyclotomicField(3)


> Z=matrix(K,[[2 * I * sqrt(3),-2],[-2,2 * I * sqrt(3)]])


> D=[4,4]


> DZ=diagonal_matrix(K,D).augment(Z) 

>  bbb=matrix(K, [[ 1, -1, 0 , 0],[ 0 , 0 , 1 ,-1]])

> zprev=DZ* bbb.transpose()

> c1=zprev.submatrix(0,0,2,1)

> c2=zprev.submatrix(0,1,2,1)

> pol=8 * (identity_matrix(K,1))

> C1D=c1 * pol.inverse() 

> W=C1D.solve_right(c2)

As said, the isotypical components 2,3 and 4 works analogously as the one above (second component). For the fifth isotypical compoonent we use a subgroup H but the primitive factor $S$ is isogenous to the image of $p_H\cdot e_5$. The command lines to obtain a basis for $S$ are as follows (in magma). After that, we move to Sagemath and it works as before.

> A5:=SubAV[5];  
> pH23:=PH(rHH[14]);  
> SS5:=Subvariety(pH23 * central_idemp[5],A5);  

SS5`bN captures the coordinates of the lattice of $S$ in the basis of the fifth isotypical component $A_5$. With this, and the period matrix of $A_5$ obtained before, we move to Ssagemath to compute the period matrix of $S$. 




