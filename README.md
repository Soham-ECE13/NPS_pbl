Introduciton;-Most of this work was performed while the second author was a Ph.D. candidate at Purdue University. A preliminary verion of this paper appears in the Proceedings of the ACM Symposium on
Access Control, Models and Technologies (SACMAT) [Li and Tripunitara 2004].
Authors’ addresses: Ninghui Li, CERIAS, Purdue University, 656 Oval Drive, West Lafayette, IN
47907; email: ninghui@cs.purdue.edu; Mahesh V. Tripunitara, Motorola Labs, IL 02, 2712, 1301 E.
Algonquin Road, Schaumburg, IL 60196; email: tripunit@motorola.com.
Permission to make digital or hard copies of part or all of this work for personal or classroom use is
granted without fee provided that copies are not made or distributed for profit or direct commercial
advantage and that copies show this notice on the first page or initial screen of a display along
with the full citation. Copyrights for components of this work owned by others than ACM must be
honored. Abstracting with credit is permitted. To copy otherwise, to republish, to post on servers,
to redistribute to lists, or to use any component of this work in other works requires prior specific
permission and/or a fee. Permissions may be requested from Publications Dept., ACM, Inc., 2 Penn
Plaza, Suite 701, New York, NY 10121-0701 USA, fax +1 (212) 869-0481, or permissions@acm.org.
C 2006 ACM 1094-9224/06/1100-0391 $5.00
ACM Transactions on Information and System Security, Vol. 9, No. 4, November 2006, Pages 391–420.
392 • N. Li and M.V. Tripunitara
European bank, resulted in an RBAC system that has around 40,000 users
and 1300 roles [Schaad et al. 2001]. In systems of such size, it is impossible for a single system security officer (SSO) to administer the entire system.
In recent years, several administrative models for RBAC have been proposed,
e.g., ARBAC97 [Sandhu et al. 1999], ARABCRA02 [Oh and Sandhu 2002], and
CL03 (Crampton and Loizou) [Crampton and Loizou 2003]. In all these models,
delegation is used to decentralize the administration tasks.
A major advantage that RBAC has over discretionary access control (DAC)
is that if an organization uses RBAC as its access-control model, then the organization (represented by the SSO in the system) has central control over
its resources. This is different from DAC, in which the creator of a resource
determines who can access the resource. In most organizations, even when a
resource is created by an employee, the resource is still owned by the organization and the organization wants some level of control over how the resource is to be shared. In most administrative models for RBAC, the SSO delegates to other users the authority to assign users to certain roles (thereby
granting those users certain access permissions), to remove users from certain roles (thereby revoking certain permissions those users have), etc. While
the use of delegation in the administration of an RBAC system greatly enhances flexibility and scalability, it may reduce the control that the organization has over its resources, thereby diminishing a major advantage RBAC
has over DAC. As delegation gives a certain degree of control to a user that
may be only partially trusted, a natural security concern is whether the organization nonetheless has some guarantees about who can access its resources.
To the best of our knowledge, the effect of delegation on the persistence of
security properties in RBAC has not been considered in the literature as
such.
In this paper, we propose to use security analysis techniques [Li et al. 2005]
to maintain desirable security properties while delegating administrative privileges. In security analysis, one views an access-control system as a statetransition system. In an RBAC system, state changes occur via administrative
operations. Security analysis techniques answer questions such as whether an
undesirable state is reachable and whether every reachable state satisfies some
safety or availability properties. Examples of undesirable states are a state in
which an untrusted user gets access and a state in which a user who is entitled
to an access permission does not get it.
Our contributions in this paper are as follows.
 We give a precise definition of a family of security analysis problems in RBAC.
In this family, we consider queries that are more general than queries that
are considered in safety analysis [Harrison et al. 1976; Koch et al. 2002a;
Lipton and Snyder 1977; Sandhu 1988].
 We show that two classes of the security analysis problems in RBAC can
be reduced to similar ones in RT[, ∩], a role-based trust-management language for which security analysis has been studied [Li et al. 2005]. The reduction gives efficient algorithms for answering most kinds of queries in
ACM Transactions on Information and System Security, Vol. 9, No. 4, November 2006.
Security Analysis in Role-Based Access Control • 393
these two classes and establishes the complexity bounds for the intractable
cases.
Our contributions are significant in that our work presents a way to capture
and represent a large class of security properties of interest in complex RBAC
systems, such as the one discussed by Schaad et al. [2001]. Our work also
shows how several kinds of these security properties can be efficiently verified.
Our establishment of complexity bounds for the intractable cases gives us a
clear understanding of the difficulty of the problems so that future work can
develop efficient heuristics. In Section 2.2, we discuss how security analysis
is used in RBAC systems, which further demonstrates the significance of our
contributions.
The rest of this paper is organized as follows. In Section 2, we define a family
of security analysis problems in RBAC and summarize our main results. We
give an overview of the results for security analysis in RT[, ∩] in Section 3.
We present the reduction from security analysis in RBAC to that in RT[, ∩]
in Section 4. Related work is discussed Section 5. We conclude with Section 6.
An appendix contains proofs not included in the main body
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
