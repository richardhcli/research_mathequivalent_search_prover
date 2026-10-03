
Heilmeier Catechism: Global Premise Retrieval over Certified Rewriting Orbits
What are you trying to do?
Given a Lean theorem T, generate certified equivalent formulations \(T_1,\dots,T_m\), retrieve a complete premise route Ri for each formulation, and jointly select
\[ (T^*,R^*)=\arg\max_{T_i,R_i} \Pr(\text{prover succeeds on }T_i\mid R_i) \]
under a fixed computational budget. Prove T*, then transport the verified proof back to T.
How is it done today, and what are its limits?
Current systems solve separate parts:
LeanSearch v2 retrieves complete premise sets, but for one fixed formulation.
Right Symmetries and FormalEvolve explore equivalent formulations, but do not model global premise retrieval.
TheoremGraph adds dependency structure, but not certified equivalence classes.
EGG-SR efficiently represents symbolic equivalence, but for symbolic regression rather than theorem proving.
Consequently, current provers remain sensitive to how a theorem is written. A poor formulation may retrieve irrelevant lemmas even when an equivalent formulation closely matches an easy Mathlib proof route.
What is new?
The retrieval query becomes the theorem’s certified equivalence class rather than one statement:
\[ T\mapsto R \quad\longrightarrow\quad [T]\mapsto(T_i,R_i). \]
The model combines:
an equivalence-invariant representation of mathematical meaning;
a formulation-aware predictor of complete proof routes;
joint selection of the formulation and route;
Lean-checked proof transport to the original goal.
The central new object is therefore the representative–premise-route pair.
Why should it succeed?
Equivalent statements frequently expose different vocabulary, structures, and library interfaces. These differences affect embedding retrieval and LLM proof generation even though they do not affect truth.
The system can exploit this variation instead of requiring one embedding to perfectly understand every possible formulation.
What difference will it make?
If successful, the method should:
increase full-route retrieval coverage;
improve end-to-end Lean proof success under equal compute;
reduce sensitivity to notation and theorem presentation;
make better use of existing Mathlib lemmas;
provide an auditable output: selected formulation, premises, proof, and transport certificate.
It could also help autoformalization choose the Lean translation that best aligns with the existing library.
What are the risks?
Equivalent formulations may not produce enough premise-route diversity.
Observed dependency sets may not represent all valid proof routes.
Exploring many formulations may cost more than it saves.
Pooling retrieved premises may add distracting noise.
Conditional or type-dependent rewrites may not form valid e-classes.
These risks require Lean-certified transformations, fixed-budget comparisons, and representative-conditioned retrieval rather than naïve premise union.
How much will it cost and how long will it take?
A credible research prototype is a four-to-six-month project using Mathlib, existing open-source retrievers and provers, and moderate GPU inference. Building reliable route annotations is likely the largest cost.
What are the examinations and next steps?
Phase 1 — Establish the phenomenon
Reproduce LeanSearch v2 and a rewriting-based prover.
Generate 5–20 certified variants for each of at least several hundred goals.
Measure variation in Recall@k, Covered@k, premise overlap, and proof success.
Phase 2 — Establish useful baselines
Compare:
original formulation;
canonicalized formulation;
pooled equivalence-class embedding;
independent retrieval over every representative;
oracle best representative.
Phase 3 — Train the joint model
Learn to rank complete (Ti,Ri) pairs, optionally using TheoremGraph dependency structure.
Final examination
Under an equal retrieval and proving budget, the joint model should outperform both fixed-form LeanSearch and independent rewriting ensembles on:
complete-route Covered@k;
end-to-end proof success;
robustness across equivalent formulations;
latency and premise-budget efficiency.
A suitable paper title is:
Invariant Goals, Equivariant Proof Routes: Global Premise Retrieval under Lean-Certified Rewriting
