---
title: 'Your Causal Variables Are Irreducibly Subjective. Embrace the Shoe Leather.'
date: 2025-07-21
permalink: /posts/2025/07/subjective-causal-variables/
tags:
  - causality
  - interpretability
  - LLMs
---

*The only honest path to causal inference about LLMs is subjective-but-reproducible variables, validated by human judgment — the same shoe-leather methodology that proved cholera was waterborne in 1854*

---

When we try to understand large language models, we like to invoke causality. And who can blame us? Causal inference comes with an impressive toolkit: directed acyclic graphs, potential outcomes, mediation analysis, formal identification results. It feels crisp. It feels reproducible. It feels like science.

But there is a precondition to the entire enterprise that we almost always skip past: you need well-defined causal variables. And defining those variables is not part of causal inference. It is prior to it — a subjective, pre-formal step that the formalism cannot provide and cannot validate.

Once you take this seriously, the consequences are severe. Every choice of variables induces a different hypothesis space. Every hypothesis space you *didn't* choose is one you can't say anything about. And the space of possible causal models compatible with any given phenomenon is not merely vast in the familiar senses — not just combinatorial over DAGs, or over the space of all possible parameterizations — but over **all possible variable definitions**, which is almost incomprehensibly vast. The best that applied causal inference can ever do is take a subjective but reproducible set of variables, state some precise structural assumptions, and falsify a tiny slice of the hypothesis space defined by those choices.

That may sound deflationary. I think embracing this subjectivity is the path to real progress — and that attempts to fully formalize away the subjectivity of variable definitions just produce the illusion of it.

## The variable definition problem

The entire causal inference stack — graphs, potential outcomes, mediation, effect estimation — presupposes that you have well-defined, causally separable variables to work with. If you don't have those, you don't get to draw a DAG. You don't get to talk about mediation. You don't get potential outcomes. You don't get any of it.

Hernán (2016) makes this point forcefully for epidemiology in "Does Water Kill? A Call for Less Casual Causal Inferences." The "consistency" condition in the potential outcomes framework requires that the treatment be sufficiently well-defined that the counterfactual is meaningful. You cannot coherently ask "does obesity cause death?" until you have specified what intervention on obesity you mean: induced how, over what timeframe, through what mechanism? Each specification defines a different causal question, and the formalism gives you no guidance on which to choose.

This is not a new insight. Freedman (1991) made the same argument in "Statistical Models and Shoe Leather." His template was John Snow's 1854 cholera investigation, the study that proved cholera was waterborne. Snow didn't fit a regression. He went door to door, identified which water company supplied each household, and used that painstaking fieldwork to establish a causal claim no model could have produced from pre-existing data. Freedman's thesis was blunt: no amount of statistical sophistication substitutes for deeply understanding how your data were generated and whether your variables mean what you think they mean. As he wrote: "Naturally, there is a desire to substitute intellectual capital for labor." His editors called this desire "pervasive and perverse." It is alive and well in LLM interpretability.

When a mechanistic interpretability researcher says "this attention head causes the model to be truthful," what is the well-defined intervention? What are the variable boundaries of "truthfulness"? In practice, we can *only* assess whether our causal models are correct by looking at them and checking whether the variable values and relationships match our subjective expectations. That is vibes dressed up as formalism.

The move toward "black-box" interpretability over reasoning paths has only made this irreducible subjectivity even more visible. The ongoing debate over whether the right causal unit is a token, a sentence, a reasoning step, or something else entirely (e.g., Bogdan et al., 2025; Lanham et al., 2023; Paul et al., 2024) is not a technical question waiting for a technical answer — it is a subjective judgment that only a human can validate by inspecting examples and checking whether the chosen granularity carves reality sensibly.[^1]

## We keep getting confused about interventions

Across 2022–2025, we've watched a remarkably consistent pattern: someone proposes an intervention to localize or understand some aspect of an LLM, and later work reveals it didn't measure what we thought.[^2] Each time, later papers argue "the intervention others used wasn't the right one." But we keep missing the deeper point: *it's all arbitrary until it's subjectively tied to reproducible human judgments.*

I've perpetuated the "eureka, we found the bug in the earlier interventions!" narrative myself. In work I contributed to, we highlighted that activation patching interventions (see Heimersheim & Nanda, 2024) weren't surgical enough, and proposed dynamic weight grafting as a fix (Nief et al., 2025). Each time we convince ourselves the engineering is progressing. But there's still a fundamentally unaddressed question: is there any procedure that validates an intervention without a human judging whether the result means what we think?

It is too easy to believe our interventions are well-defined because they are granular, forgetting that granularity is not validity.[^3]

## Shoe leather for LLM interpretability

Freedman's lesson was that there is no substitute for shoe leather — for going into the field and checking whether your variables and measurements actually correspond to reality. So what is shoe leather for causal inference about LLMs?

I think it has three components. First, **embrace LLMs as a reproducible operationalization of subjective concepts.** A transcoder feature, a swapped sentence in a chain of thought, a rephrased prompt — these are engineering moves, not variable definitions. "Helpfulness" is a variable. "Helpfulness as defined by answering the user's request without increasing liability to the LLM provider" is another, distinct variable. Describe the attribute in natural language, use an LLM-as-judge to assess whether text exhibits it, and your variable becomes a measurable function over text — reproducible by anyone with the same LLM and the same concept definition.

Second, **do systematic human labeling of all causal variables and interventions** to verify they actually match what you think they should. If someone disagrees with your operationalization, they can audit a hundred samples and refine the natural language description. This is the shoe leather: not fitting a better model, but checking — sample by sample — whether your measurements mean what you claim.

Third — and perhaps most importantly — **publish the labeling procedure, not just the code.** A reproducible natural language specification of what each variable means, along with the human validation that confirmed it, is arguably more valuable than a GitHub repo. It is what lets someone else pick up your variables, contest them, refine them, and build on your falsifications rather than starting from scratch.

Variable definitions are outside the scope of causal inference. Publishing how you labeled them matters more than publishing your code.

## Trying to practice what I'm preaching

RATE (Reber, Richardson, Nief, Garbacea, & Veitch, 2025) came out of trying to do this in practice — specifically, trying to scale up subjective evaluation of traditional steering approaches in mech interp. We knew from classical causal inference that we needed counterfactual pairs to measure causal effects of high-level attributes on reward model scores. Using LLM-based rewriters to generate those pairs was the obvious move, but the rewrites introduced systematic bias. Fixing that bias — especially without having to enumerate everything a variable *can't* be — turned out to be a whole paper. The core idea: rewrite twice, and use the structure of the double rewrite to correct for imperfect counterfactuals.

## The punchline

Building causal inference on top of subjective-but-reproducible variables is harder than it sounds, and there's much more to do. But I believe the path is clear, even if it's narrow: subjective-yet-reproducible variables, dubious yet precise structural assumptions, honest statistical inference — and the willingness to falsify only a sliver of the hypothesis space at a time.

Every causal variable is a subjective choice — and because the space of possible variable definitions is vast, so is the space of causal hypotheses we'll never even consider. No formalism eliminates this. No amount of engineering granularity substitutes for a human checking whether a variable means what we think it means. The best we can do is choose variables people can understand, operationalize them reproducibly, state our structural assumptions precisely, and falsify what we can. That sliver of real progress beats a mountain of imagined progress every time.

---

[^1]: Causal representation learning (e.g., Schölkopf et al., 2021) doesn't help here either. Weakening "here is the DAG" to "the DAG belongs to some family" is still a structural assertion made before any data is observed.

[^2]: Two threads illustrate the pattern. First: ROME (Meng et al., 2022) used causal tracing to localize factual knowledge to specific MLP layers — a foundational contribution. Hase et al. (2023) showed the localization results didn't predict which layers were best to edit. Wang & Veitch (2025) showed that optimal edits at *random* locations can be as effective as edits at supposedly localized ones. Second: DAS (Geiger, Wu, Potts, Icard, & Goodman, 2024) found alignments between high-level causal variables and distributed neural representations via gradient descent. But Makelov et al. (2023) demonstrated that subspace patching can produce "interpretability illusions" — changing output via dormant pathways rather than the intended mechanism — to which Wu et al. (2024) argued these were experimental artifacts. Causal abstraction has real theoretical foundations (e.g., Beckers & Halpern, 2019; Geiger et al., 2025; Xia & Bareinboim, 2024), but it cannot eliminate the subjectivity of variable definitions — only relocate it. Sutter et al. (2025) show that with unconstrained alignment maps, any network can be mapped to any algorithm, rendering abstraction trivially satisfied. The linearity constraints practitioners impose to avoid this are themselves modeling choices — ones that can only be validated by subjective judgment over the data.

[^3]: There is also a formal issue lurking here: structural causal models require exogenous noise, and neural network computations are deterministic. Without extensions like Beckers, Halpern, & Hitchcock's (2023) "Causal Models with Constraints," we don't even have a well-formed causal model at the neural network level — so what are we abstracting between?

---

**References**

Beckers, S. & Halpern, J. Y. (2019). Abstracting causal models. *AAAI-19*.

Beckers, S., Halpern, J. Y., & Hitchcock, C. (2023). Causal models with constraints. *CLeaR 2023*.

Bogdan, P. C., Macar, U., Nanda, N., & Conmy, A. (2025). Thought anchors: Which LLM reasoning steps matter? *arXiv:2506.19143*.

Freedman, D. A. (1991). Statistical models and shoe leather. *Sociological Methodology*, 21, 291–313.

Geiger, A., Wu, Z., Potts, C., Icard, T., & Goodman, N. (2024). Finding alignments between interpretable causal variables and distributed neural representations. *CLeaR 2024*.

Geiger, A., Ibeling, D., Zur, A., et al. (2025). Causal abstraction: A theoretical foundation for mechanistic interpretability. *JMLR*, 26, 1–64.

Hase, P., Bansal, M., Kim, B., & Ghandeharioun, A. (2023). Does localization inform editing? *NeurIPS 2023*.

Heimersheim, S. & Nanda, N. (2024). How to use and interpret activation patching. *arXiv:2404.15255*.

Hernán, M. A. (2016). Does water kill? A call for less casual causal inferences. *Annals of Epidemiology*, 26(10), 674–680.

Lanham, T., et al. (2023). Measuring faithfulness in chain-of-thought reasoning. *Anthropic Technical Report*.

Makelov, A., Lange, G., & Nanda, N. (2023). Is this the subspace you are looking for? *arXiv:2311.17030*.

Meng, K., Bau, D., Andonian, A., & Belinkov, Y. (2022). Locating and editing factual associations in GPT. *NeurIPS 2022*.

Nief, T., et al. (2025). Multiple streams of knowledge retrieval: Enriching and recalling in transformers. *arXiv:2506.20746*.

Paul, D., et al. (2024). Making reasoning matter: Measuring and improving faithfulness of chain-of-thought reasoning. *Findings of EMNLP 2024*.

Reber, D., Richardson, S., Nief, T., Garbacea, C., & Veitch, V. (2025). RATE: Causal explainability of reward models with imperfect counterfactuals. *ICML 2025*.

Schölkopf, B., Locatello, F., Bauer, S., et al. (2021). Toward causal representation learning. *Proceedings of the IEEE*, 109(5), 612–634.

Sutter, D., Minder, J., Hofmann, T., & Pimentel, T. (2025). The non-linear representation dilemma. *arXiv:2507.08802*.

Wang, Z. & Veitch, V. (2025). Does editing provide evidence for localization? *arXiv:2502.11447*.

Wu, Z., Geiger, A., Huang, J., et al. (2024). A reply to Makelov et al.'s "interpretability illusion" arguments. *arXiv:2401.12631*.

Xia, K. & Bareinboim, E. (2024). Neural causal abstractions. *AAAI 2024*, 38(18), 20585–20595.
