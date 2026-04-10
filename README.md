2. PROBLEM DEFINITION AND MAIN RESULTS
In Li et al. [2005], an abstract version of security analysis is defined in the
context of trust management. In this section, we restate the definition in the
context of general access-control schemes.
Definition 1. (Access-Control Schemes) An access-control scheme is modeled as a state-transition system , Q, , , in which  is a set of states, Q is
a set of queries,  is a set of state-change rules, and  :  × Q → {true, false}
is called the entailment relation, determining whether a query is true or not in
a given state. A state, γ ∈ , contains all the information necessary for making access-control decisions at a given time. When a query, q ∈ Q, arises from
an access request, γ q means that the access corresponding to the request q
is granted in the state γ , and γq means that the access corresponding to q
is not granted. One may also ask queries other than those corresponding to a
specific request, e.g., whether every principal that has access to a resource is
an employee of the organization. Such queries are useful for understanding the
properties of a complex access-control system.
A state-change rule, ψ ∈ , determines how the access-control system
changes state. Given two states γ and γ1 and a state-change rule ψ, we write
γ 	→ψ γ1 if the change from γ to γ1 is allowed by ψ, and γ ∗
	→ψ γ1 if a sequence
of zero or more allowed state changes leads from γ to γ1. If γ ∗
	→ψ γ1, we say
that γ1 is ψ-reachable from γ , or simply γ1 is reachable, when γ and ψ are clear
from the context.
An example of an access-control scheme is the HRU scheme, that is derived
from the work by Harrison et al. [1976]. The HRU scheme is based on the
access-matrix model [Graham and Denning 1972; Lampson 1971]. We assume
the existence of three countably infinite sets: S, O, and A, which are the sets of
ACM Transactions on Information and System Security, Vol. 9, No. 4, November 2006.
394 • N. Li and M.V. Tripunitara
all possible subjects objects and access rights. We assume further that S ⊆ O.
In the HRU scheme:
  is the set of all possible access matrices. Formally, each γ ∈  is identified
by three finite sets, Sγ ⊂ S, Oγ ⊂ O, and Aγ ⊂ A, and a function Mγ []:
Sγ ×Oγ → 2Aγ , where Mγ [s, o] gives the set of rights s has over o. An example
of a state, γ , is one in which Sγ = 
Admin
, Oγ = 
employeeData
∪ Sγ , Aγ =
{own, read}, and Mγ [Admin, Admin] = ∅, and Mγ [Admin, employeeData] = 
own, read
. In this state, two objects exist, of which one is a subject, and the
system is associated with the two rights, own and read.
 Q is the set of all queries of the form: a ∈ [s, o], where a ∈ A is a right, s ∈ S
is a subject, and o ∈ O is an object. This query asks whether the right a exists
in the cell corresponding to subject s and object o.
 The entailment relation is defined as follows: γ a ∈ [s, o] if, and only if,
s ∈ Sγ , o ∈ Oγ , and a ∈ Mγ [s, o]. For example, let the query q1 be read ∈
M[Admin, employeeData]. and the query q2 be own ∈ M[Admin, Admin] Then,
for the state, γ , discussed above, γq1 and γ q2.
 Each state-transition rule ψ is given by a set of commands. Given ψ,
the change from γ to γ1 is allowed if there exists command in ψ such that the
execution of the command in the state γ results in the state γ1. An example
of ψ is the following set of commands.
command createObject(s, o) command grant a(s, s
, o)
create object o if own ∈ [s, o]
enter own into [s, o] enter a into [s
, o]
The set of queries is not explicitly specified in Harrison et al. [1976]. It is
conceivable to consider other classes of queries, e.g., comparing the set of all
subjects that have a given right over a given object with another set of subjects.
In our framework, HRU with different classes of queries can be viewed as
different schemes.
Definition 2. (Security Analysis in an Abstract Setting) Given an accesscontrol scheme , Q, , , a security analysis instance takes the form
γ, q, ψ, , where γ ∈  is a state, q ∈ Q is a query, ψ ∈  is a state-change
rule, and  ∈ {∃, ∀} is a quantifier. An instance γ, q, ψ, ∃ asks whether there
exists γ1 such that γ ∗
	→ψ γ1 and γ1 q. When the answer is affirmative, we say
q is possible (given γ and ψ). An instance γ, q, ψ, ∀ asks whether for every γ1
such that γ ∗
	→ψ γ1, γ1  q. If so, we say q is necessary (given γ and ψ).
For our example HRU scheme from above, adopt γ as the start state. In
γ , there is only one subject (namely, Admin) and the access matrix is empty.
The system is associated with the two rights, own and r. Let the query q be
r ∈ M[Alice, employeeData] for Alice ∈ S and employeeData ∈ O. Let the statechange rule ψ be the set of two commands createObject and grant r. Then, the
security analysis instance γ, q, ψ, ∃ is true. The reason is that although in
the start state γ , Alice does not have the r right over the object employeeData,
there exists a reachable state from γ in which she has such access. The security
ACM Transactions on Information and System Security, Vol. 9, No. 4, November 2006.
Security Analysis in Role-Based Access Control • 395
analysis instance γ, q, ψ, ∀ is false, as there exists at least one state reachable
from γ (γ itself) that does not entail the query.
Security analysis generalizes safety analysis. As we discuss in the following
section, with security analysis we can study not only safety, but also several
other interesting properties, such as availability and mutual exclusion
