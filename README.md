Position: The Party That Owes Erasure Cannot Re-Execute the Verification

Verifier Access Tier as a Missing Parameter of Deletion and Privacy Guarantees

Supplementary repository for an anonymous submission to IEEE SaTML 2027.

What is in this repository, and what is not

There is no experimental code, no dataset, and no trained model in this repository, and none is being withheld.

This is a position paper. It reports no experiments, introduces no models, trains and fine-tunes nothing, and processes no personal data. Its argument is a re-reading of two existing literatures — machine unlearning and the empirical auditing of differential privacy — along an axis those literatures do not currently report: the access tier a verifier must hold in order to re-execute the check. Every empirical claim in the paper is a claim about a published result, attributed to that result and checkable against it; the paper introduces no measurement of its own.

This repository exists because the submission form requires an artifact URL. Rather than leave the field empty, it records, in a form that can be read and reused independently of the paper, the one object the paper does introduce.

What the paper does introduce

A deletion evidence package: an artifact produced at training time by the party that holds the access, and consumed at audit time by the party that holds the duty. The paper specifies it in full in its body; it is restated here so that it can be adopted, criticised or extended without the paper.

Soundness condition

Let P be a provider that has executed a claimed deletion, V a verifier at access tier t chosen by the deployer, and A a provider strategy that produces the package while retaining the influence of the target records.

A package is sound at tier t against A with power β at false-positive rate α if V, executing only capabilities available at tier t, and without P learning which probes will be scored, rejects a package produced under A with probability at least β, while rejecting an honest package with probability at most α.

Three consequences follow immediately:

Soundness is relative to a named provider strategy, not absolute.
The tuple (α, β, t, A) is part of the claim and must appear on the face of the artifact.
A package with unreported α is not a weak package — it is an unmeasured one.

The definition is not compositional: soundness against A₁ and against A₂ does not imply soundness against a strategy mixing them. Quantifying over a class of strategies rather than a list is open.

The four necessary properties
	Property	What it requires
P1	Algorithm-attested, not artifact-asserted	The claim is that a specified algorithm ran on a specified data partition, committed to before the deletion request arrived — not that the resulting model has some property.
P2	Adversary-relative, with a declared threat model	The package states which adversary the claim holds against (query-only, representation access, in-context re-supply), which elicitation instruments were used, and how those instruments behave on control content the model never saw.
P3	Third-party executable, and not prepared for	The evidence is re-executable by someone other than the provider, under a protocol in which the provider does not learn which probes will be scored.
P4	Revoked by model updates	The claim binds to a model version and expires. Continuous fine-tuning, retrieval augmentation and vendor-side base-model updates each invalidate it.

These are proposed as necessary, not sufficient. Constructing a package that satisfies them is open work, and the paper says so.

Fields of the package

Every row below is either a commitment made before the request arrived, a measurement with a declared instrument and declared error behaviour, or a scope limit. No row is an assurance.

Field	Content
Scope	Model version identifier; which stage the claim covers (pre-training, fine-tune, adapter, cache); explicit list of derivatives in scope
Procedure	Identifier of the deletion algorithm executed, bound to a pre-existing commitment to partition and schedule (P1)
Adversary	Declared strategy A: query-only, representation access, in-context re-supply (P2)
Measurement	Empirical bound from a named auditing procedure, with the access tier at which it was obtained
Error behaviour	α on control content never present in training; β against A (P2)
Verifier	Who may re-execute the check, under what protocol, and whether the provider learns the probe set (P3)
Expiry	Version binding and the events that invalidate the claim (P4)
Known gaps	Loci the claim does not cover, stated affirmatively

The last row is the one expected to be resisted and the one that matters most. A package that cannot say what it does not cover is indistinguishable from vendor self-disclosure, and will be relied upon precisely where it is weakest.

Access tiers

The classification used throughout the paper. It is not a total order over organisations: it is a coarsening of a product, training stage × capability, and the same organisation occupies different tiers at different stages. A deployer that runs its own fine-tune holds T3 capability over the fine-tuning stage while holding T0 or T1 over the base model.

Tier	Capability
T0	Black-box, output only. Generated text or labels. No log-probabilities, no seeds, no weights, no ability to train.
T1	Black-box with scores. T0 plus token-level log-probabilities or top-k. Enough for likelihood-based probes; not enough to read parameters.
T2	White-box, final model. Weights or adapters available; training trajectory not. The "hidden state" setting.
T3	Training-time. The verifier observes or influences the run: intermediate iterates, gradient insertion, data partitioning, initialisation.

verifier_tiers.csv in this repository is a machine-readable transcription of Table I of the paper, which classifies representative published results by the verifier capability each one requires, and separates results that establish a property from instruments that only refute one. It is a transcription for convenience, not a new result: every row is derived from the cited work's own description of its method, and the citation key is given so that each row can be checked against its source.

Reproducibility

There is nothing to run. The paper's only checkable output is its reading of published work, and every such reading is reproducible in the ordinary way: open the cited paper and compare. The reference list gives an arXiv identifier, DOI, official journal citation or URL for every entry.

The literature-discovery step was assisted by a large language model, which introduces a coverage bias that cannot be certified away; this is stated in the paper's "LLM usage considerations" section, along with how it was mitigated. The verification step — checking each retrieved item against its own record — is fully reproducible, because every item is identified.

License

The contents of this repository are released under CC BY 4.0. The deletion evidence package specification is intended to be adopted, modified and criticised freely.
