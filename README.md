# Period-matrices
Magma code for Period matrices for Abelian varieties
* Grupo800-980.mgm. Contains the symplectic representation for G(800,980) on genus 101 with signature (0;8,8,2). Obtained with polyNoZ.m in https://github.com/rojas-ani/magma-routines. Outputs: 
  - a, b: Symplectic representation of the generators of G(800,980) of order 8 (corresponding to elements fixing points). The element of order 2 is ab.
  - G: The image of G(800,980) in Sp(202,Z)
  - H: The symplectic image of the unique abelian subgroup of G(800,980) of order 100.
* ActionGSubvariety.mgm. Code for Theorem 4.1. Contains several functions:
  - RestrictedRepresentation(L,b): L is expected to be a set of generators of G given as symplectic matrices in some basis B of an ambient ppav A, and b the coordinate matrix of a symplectic basis of a G-subvariety with respect to the same simplectic basis B. The output is a list of the same generators as in L but now represented on the basis b.
  - InducedPolarization(e): e is an idempotent represented in a symplectic basis for a ppav A. The output are bN, Jind: the coordinate matrix of a symplectic basis of Im(a) and the induced polarization on it.
  - PH(H): H is a subgroup of G (symplectic). The output is the idempotent associated to H, $p_H=1/|H| \sum_{h\in H} h$.
  - ActionGSubJacobian(L,H): L set of symplectic generators of G, H a symplectic subgroup of G. If the image of $p_H$ is G-invariant, it computes the action of G restricted to it. The output is a record with ph the idempotent $p_H$, bN the coordinates of a symplectic basis for the lattice of $A_H:=Im(p_H)$, Jind the induced polarization on $A_H$, LH the action of G restricted to $A_H$. Otherwise gives *error*.
  - ActionGSubvariety(L,e): Same as above for $A^e=Im(e)$, provided it is a G-subvariety. For instance, when e is a central idempotent (given as a symplectic matrix).
  - IsotypicalFactorAll(G): G as a symplectic group. It computes all the central idempotents, the dimensions of the isotypical factors and the primitive ones (in GAD). It also identify which ones are positive dimensional, among other information.
  - IsotypicalFactor(G,tb,rep): Input: G (symplectic), tb (character table of G), rep (the number of the representation whose isotypical component you want to study). The output is a record with the dimension of the isotipical component corresponding to that complex irreducible representation, the dimension of the primitive factor (in GAD), the central idempotent, the Schur index.

**Example** Section 7: A genus 101 curve with completely decomposable Jacobian.
> load "Grupo800-980.mgm";  
> load "ActionGSubvariety.mgm";  
> JH:=ActionGSubJacobian([a,b],H);  
> Attach("polyDZ.m");  
> z:=MoebiusInvariantDZ([10,10], JH`LH);  

