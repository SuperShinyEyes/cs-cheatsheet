=====
Logic
=====

Logical Connectives
===================
``NOR`` and ``NAND`` gates are sufficient to express any Boolean function, i.e., they are `functionally complete`_. If you want to verify that a set of operators is functionally complete, show that can be expressed with either ``NOR`` or ``NAND``. `Read Stackoverflow for more detail`_.

.. figure:: /images/ai/Logical_connectives_Hasse_diagram.svg
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Logical connectives Venn diagram >


.. _`functionally complete`: https://en.wikipedia.org/wiki/Functional_completeness
.. _`Read Stackoverflow for more detail`: https://stackoverflow.com/a/33161222/3067013

-----------------------------------------------------------------------

Logical Reasoning
=================

.. figure:: /images/ai/declarative.svg
   :align: center
   :alt: alternate text
   :figclass: align-center

   < The illustration of declarative problem solving. The logical approach provides the foundation for it. [1]_ >

----------------------------------------

Language of Propositional Logic
===============================

Syntax for Propositional Formulas
#################################
* negation :math:`\neg` ("not")
* conjunction :math:`\land` ("and")
* disjunction :math:`\lor` ("or")
* implication :math:`\rightarrow` ("if ... then")
* equivalence :math:`\leftrightarrow` ("if and only if" or "bi-implication")

Examples
^^^^^^^^
* rainy :math:`\rightarrow` wet: If it rains, then it's wet.
* :math:`\neg` dry :math:`\lor` :math:`\neg` swim: One can't be dry while he is swimming and vice versa.


Definition – formula
####################

1. Every atomic formula is a **formula**.
2. :math:`\text{If $\alpha$ and $\beta$ are formulas, then also $(\neg\alpha), (\alpha\lor\beta), (\alpha\land\beta), (\alpha\rightarrow\beta), (\alpha\leftrightarrow\beta) $ are}` **formulas**.


Definition – subformula
#######################

The subformulas of a formula are defined recursively as follows.

1. The only subformula of an atomic proposition **a** is **a** it self.
2. The subformulas of :math:`\not \alpha` are :math:`\not \alpha` and all the subformulas of :math:`\alpha`.
3. If :math:`*` is any of the connectives :math:`\lor,\land,\rightarrow,\leftrightarrow,` the subformulas of :math:`(\alpha * \beta)` are :math:`(\alpha * \beta)` and all the subformulas of :math:`\alpha` and :math:`\beta`.

Examples
^^^^^^^^
The subformulas of the formula :math:`swim \rightarrow \neg dry` are the formula itself, :math:`swim`, :math:`\neg dry` and :math:`dry`.

Example – Formulas from natural language
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Consider the following statement in natural language:

*If the file is too big, then it is compressed or removed.*

For the sake formalization, we can identify the following atomic statements/formulas:

1. b: The file is too big.
2. c: The file is compressed.
3. r: The file is removed.

By substituting these statements into the original statement, we may derive a preliminary formalization that mixes natural language with the formal one:

.. math::
  \text{If b, then c or r}

Finally the formula is obtained by identifying the connectives and by incorporating them into the expression:

.. math::
  b \rightarrow c \lor r

Example for Terminologies
#########################

.. figure:: /images/ai/sudoku-small.svg
   :align: center
   :alt: alternate text
   :figclass: align-center

   < 2x2 sudoku example [1]_ >

We can study the above sudoku and reason that <2,1> can only contain 2. This can be expressed in a formal logical calculus as a disjunction

.. math::
  {val}(1,1,2)\lor {val}(1,2,2)\lor {val}(2,1,2)\lor {val}(2,2,2) \\

  \text{(i.e., 2 has to be in either in one of the 2x2 grid.)}

The clue 2 at coordinates ⟨1,3⟩ and constraints for the first column (where x=1) contribute the following formulas

.. math::
  \begin{array}{ccc}
  {val}(1,3,2), &
  \neg{val}(1,2,2)\lor\neg{val}(1,3,2), &
  \neg{val}(1,1,2)\lor\neg{val}(1,3,2).
  \end{array} \\

  \text{(i.e., 2 cannot be placed simultaneously in the cells ⟨1,2⟩
 and ⟨1,3⟩.)}


Truth tables
############

.. math::
  \begin{array}{|c|c|c|} \hline  \alpha & \beta & (\alpha\land\beta) \\ \hline \hline  F & F & F \\ \hline  F & T & F \\ \hline  T & F & F \\ \hline  T & T & T \\ \hline  \end{array}

.. math::
  \begin{array}{|c|c|c|} \hline  \alpha & \beta & (\alpha\lor\beta) \\ \hline \hline  F & F & F \\ \hline  F & T & T \\ \hline  T & F & T \\ \hline  T & T & T \\ \hline  \end{array}

.. math::
  \begin{array}{|c|c|c|} \hline  \alpha & \beta & (\alpha\rightarrow\beta) \\ \hline \hline  F & F & T \\ \hline  F & T & T \\ \hline  T & F & F \\ \hline  T & T & T \\ \hline  \end{array}

.. math::
  \begin{array}{|c|c|c|} \hline  \alpha & \beta & (\alpha\leftrightarrow\beta) \\ \hline \hline  F & F & T \\ \hline  F & T & F \\ \hline  T & F & F \\ \hline  T & T & T \\ \hline  \end{array}

.. math::
  \begin{array}{|c|c|c|} \hline  \alpha & \beta & (\alpha\oplus\beta) \\ \hline \hline  F & F & F \\ \hline  F & T & T \\ \hline  T & F & T \\ \hline  T & T & F \\ \hline  \end{array}

Models, Satisfiability and Unsatisfiability
###########################################
Satisfaction is a relationship between specific sentences and specific truth assignments. [2]_

* A sentence is *valid* if and only if it is satisfied by *every* truth assignment.

  * e.g. :math:`(p \lor \neg p)`
  * *satisfiable*

* A sentence is unsatisfiable if and only if it is not satisfied by any truth assignment.

  * e.g. :math:`(p \land \neg p)`
  * *falsifiable*

* A sentence is contingent if and only if there is some truth assignment that satisfies it and some truth assignment that falsifies it.

  * e.g. :math:`(p \land q)`
  * *satisfiable* AND *falsifiable*.


Definition – model
^^^^^^^^^^^^^^^^^^
Let :math:`\alpha` be a formula and :math:`\Sigma` a set of formulas. A truth assignment :math:`v` is called

1. a **model** of :math:`\alpha`, if :math:`v \vDash \alpha`, and
2. a **model** of :math:`\Sigma`, if :math:`v \vDash \Sigma`, i.e., :math:`v \vDash \beta` for every formula :math:`\beta \in \Sigma`

Logical entailment
##################
We say that a sentence :math:`\phi` logically entails a sentence :math:`\psi` (written :math:`\phi` ⊨ :math:`\psi`) if and only if every truth assignment that satisfies :math:`\phi` also satisfies :math:`\psi`. For example, the sentence :math:`p` logically entails the sentence :math:`(p \lor q)`. Since a disjunction is true whenever one of its disjuncts is true, then :math:`(p \lor q)` must be true whenever :math:`p` is true.

Logical consistency
###################
A sentence :math:`\phi` is consistent with a sentence :math:`\psi` if and only if there is a truth assignment that satisfies both :math:`\phi` and :math:`\psi`. For example, the sentence :math:`(p \lor q)` is consistent with the sentence :math:`(p \land q)`. However it is NOT consistent with :math:`(\neg p \land \neg q)` [3]_

While consistency and entailment are very similar they don't entail each other.

Transformation Rules
####################

.. math::
  \begin{align}
  \text{1. } & \alpha\leftrightarrow\beta \Longrightarrow       (\alpha\rightarrow \beta)\land(\beta\rightarrow \alpha) \Longrightarrow       (\neg\alpha\lor\beta)\land(\neg\beta\lor\alpha) \\
  \text{2. } & \alpha\rightarrow \beta \Longrightarrow       \neg\alpha\lor\beta           \\
  \text{3. } & \neg(\alpha\lor\beta) \Longrightarrow       \neg\alpha\land\neg\beta          \\
  \text{4. } & \neg(\alpha\land \beta) \Longrightarrow       \neg\alpha\lor\neg\beta          \\
  \text{5. } & \neg\neg\alpha \Longrightarrow \alpha          \\
  \text{6. } & \alpha\lor(\beta\land \gamma) \Longrightarrow       (\alpha\lor\beta)\land(\alpha\lor\gamma)          \\
  \text{7. } & (\alpha\land \beta)\lor\gamma \Longrightarrow       (\alpha\lor\gamma)\land(\beta\lor\gamma)          \\
  \text{8. } & \alpha\land(\beta\lor\gamma) \Longrightarrow       (\alpha\land\beta)\lor(\alpha\land\gamma)          \\
  \text{9. } & (\alpha\lor\beta)\land\gamma \Longrightarrow       (\alpha\land\gamma)\lor(\beta\land\gamma)          \\
  \end{align}

To transform a propositional formula into a CNF or DNF, follow the below steps:

1. Remove equivalences :math:`(\leftrightarrow)`
2. Remove implications :math:`(\rightarrow)`
3. Push negations inside until negations :math:`(\neg)` occur as parts of negative literals only.
4. Organize conjunctions outside disjunctions (CNF) or disjunctions outside conjunctions (DNF).


Conjunctive Normal Form(CNF)
############################
A statement is in conjunctive normal form if it is a conjunction (sequence of ANDs) consisting of one or more conjuncts, each of which is a disjunction (OR) of one or more literals. Examples of conjunctive normal forms include [4]_

.. math::
  A \\
  (A \lor B) \land (\neg A \lor C)  \\
  A \lor B   \\
  A \land (B \lor C)  \\

Example
^^^^^^^
Transform the formula :math:`\neg(p\land q)\leftrightarrow(r\land s)` into CNF.

.. math::
  \begin{gather} 
  \text{1. Replace the equivalance using, $P \leftrightarrow Q \Longleftrightarrow (P \lor \neg Q) \land (\neg P \lor Q)$.} \\
  \big[ \neg(p\land q) \lor \neg (r\land s) \big] \land \big[ p\land q) \lor (r\land s) \big] \\
  \text{2. Applythe distributed law} \\
  \big[ \neg(p\land q) \lor \neg (r\land s) \big] \land 
  \big[ (p\lor r) \land (p\lor s) \land (q\lor r) \land (q \lor s) \big] \\  
  \text{3. Apply De Morgan's law} \\
  \big( \neg p \lor \neg q \lor \neg r\lor \neg s \big) \land 
  \big( (p\lor r) \land (p\lor s) \land (q\lor r) \land (q \lor s) \big) \\  
  \end{gather} 

Disjunctive normal form (DNF)
#############################
A formula :math:`\alpha` is in disjunctive normal form (DNF) if and only if has the form :math:`{\beta_1}{\lor}\cdots{\lor}{\beta_n}` where :math:`n\geq 0` and each disjunt :math:`\beta_i` is a cube. Example:

.. math::
  (\neg {fire}\land\neg {alarm})\lor( {fire}\land {alarm})


----------------------------------------

Answer Set Programming(ASP)
===========================
ASP is a declarative programming method emphasizing on **what** to be computed rather than **how** to compute.

.. figure:: /images/ai/declarative1.svg
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Formulas as rules and satisfying assignments as answer sets. Source: Aalto AI [1]_ >

Answer sets
###########
1. The set is closed under the rules of the program.
2. If some particular atom belongs to the set, then there is at least one supporting rule instance having the atom in question as its head and the body of the rule is satisfied by the set. In other words, atoms cannot be true without a reason.
3. The set is minimal in this sense. In other words, atoms are false by default.


Positive programs
#################
A positive rule is is an expression of the form

.. math::
  a \leftarrow b_1, \cdots ,b_n

where the head atom :math:`a` can be inferred if the body atoms :math:`b_1, \cdots ,b_n` have been inferred by other rules in the program. A rule with an empty body (:math:`n=0`) is called a fact.

.. topic:: Definition
  The (unique) answer set of a positive program is the least set :math:`S` of ground atoms which is closed under the ground instances of its rules:

  1. If there is a ground instance :math:`h(t)\leftarrow b_1(t_1) \cdots b_n(t_n)` of some rule in the program such that :math:`b_1(t_1)\in S, \ldots, b_n(t_n)\in S`, then also :math:`h(t) \in S`
  2. If some other set :math:`S'\subseteq S` is closed in this way, then :math:`S' = S`

-------------------------------

.. comments

.. rubric:: Reference

.. [1] https://mycourses.aalto.fi/course/view.php?id=16956
.. [2] http://intrologic.stanford.edu/sections/section_03_01.html
.. [3] http://intrologic.stanford.edu/sections/section_03_05.html
.. [4] http://mathworld.wolfram.com/ConjunctiveNormalForm.html