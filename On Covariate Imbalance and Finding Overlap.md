# On Covariate Imbalance and Finding Overlap (Fall 2020)
* Machine Learning for Healthcare (DS-GA.3001-005/CSCI-GA.3033-083)
* Notes pertaining to final project

## General Notes
* Heterogeneous treatment effects; estimating CATE
* Subclassification & matching & propensity score matching (2 problems: feature selection, small matched samples)
* Curse of dimensionality: too much subclassification / conditioning => worse sample size & accuracy
* Consistent vs unbiased estimator
* Two requirements for splitting: balance (similar sample size) & accuracy difference?
* EMSE (expected mean squared error for treatment effects): splitting criterion
* Overfitting: solve via honesty (splitting subsample vs estimating subsample)
* Double-sample tree & propensity tree
* https://www.causalflows.com/causal-tree-learning/
* https://cds.nyu.edu/wp-content/uploads/2014/04/causal-and-data-science-and-BART.pdf -- Jennifer Hill
* https://scholar.princeton.edu/sites/default/files/bstewart/files/lundberg_methods_tutorial_reading_group_version.pdf -- lecture slides
* > Bayesian additive regression trees (BART) provides a flexible approach to fitting a variety of regression models while avoiding strong parametric assumptions. The sum-of-trees model is embedded in a Bayesian inferential framework to support uncertainty quantification and provide a principled approach to regularization through prior specification.
* Positivity violation at a later time possible?!
* https://humboldt-wi.github.io/blog/research/applied_predictive_modeling_19/matching_methods/ -- applications (and details) of coarsened exact matching + propensity matching + genetic matching
* Mahalanobis distance matching; estimate propensity score (logistic reg; RF; boosted trees); genetic matching; Matching Frontier (algo to prune as least as possible); D-AEMR (Dynamic Almost Exact Matching with Replacement; matches important subset of covariates still in high-dimensional space)
* Opossum: Python library for synthetic data generation
* https://digitalcommons.library.tmc.edu/cgi/viewcontent.cgi?article=1084&context=uthsph_dissertsopen -- U of Texas dissertation
* Doubly robust: need either μ(t, X) or e() to be correctly specified (= consistently estimated?) / not necessarily both
* Additive trees: overfitting (remedied by hyperparameters like tree depth & shrinkage); more thorough control for confounding than parametric models
* BART: a la G-computation then?!; Y = f(t, X) + ε = g(t, X; ·) + ... + g(·) + ε
* Athey's causal trees: seem to be not exactly G-computation; each leaf contains info on ATE?
* 3 ways to use tree-based: propensity score (JCART); Y (BART); ATE (Athey's)
* Cox proportional hazards model: effect of covariate is multiplicative on hazard function
* Estimand of interest (for survival analysis & CI) could be: restricted mean survival time, risk difference (ATE?), relative risk
* Targeting phase of TMLE: reduces residual bias via perturbation (hyperparameter)
* 3 ways to regularize a tree: limit max depth; ensembles / bagging; set stricter stopping criterion on when to split
* "Dealing with limited overlap in estimation of average treatment effects" by Crump et al. (2009) -- "generally can discard outside of [0.1, 0.9]" (see page 9)
* https://arxiv.org/pdf/1508.06948.pdf -- "Propensity Score Matching and Subclassification in Observational Studies with Multi-level Treatments"; related to Crump; idea: efficiency bound (under homoscedasticity) -> try to minimize it -> cut off propensity score at certain thresholds (see page 16)
* Decision rule learning; decision list (~= cond do)
* IPW intuition: weighting generalizes matching
* Instrumental variable (IV): Z where Z -> T -> Y (e.g. blood type -> organ transplant -> death within a month); encouragement
* IV assumptions: association with treatment; exclusion restriction (no direct/indirect effect to Y outside of T); monotonicity (no defier <-> encouragement)
* Generalized propensity score (GPS): when treatment is not binary
* E[X I_A] = ① \sum x P(x, a); ② E[X | A] P(A) -- via law of total expectation (= law of iterated expectations)
* Weighted logistic regression possible (not just WLS)?!
* https://arxiv.org/pdf/1706.09523.pdf -- BCF (& ps-BART) = BART + propensity score (to mitigate RIC = regularization-induced confounding)
* > A potential drawback of the weighting approaches is that, as with Horvitz-Thompson estimation, the variance can be very large if the weights are extreme (i.e., if the estimated propensity scores are close to 0 or 1). If the model is correctly specified and thus the weights are correct, then the large variance is appropriate. However, a worry is that some of the extreme weights may be related more to the estimation procedure than to the true underlying probabilities. Weight trimming, which sets weights above some maximum to that maximum, has been proposed as one solution to this problem (Potter, 1993; Scharfstein et al., 1999). However, there is relatively little guidance regarding the trimming level. Because of this sensitivity to the size of the weights and potential model misspecification, more attention should be paid to the accuracy of propensity score estimates when the propensity scores will be used for weighting vs. matching (Kang and Schafer, 2007). Another effective strategy is doubly-robust methods (Bang and Robins, 2005), which yield accurate effect estimates if either the propensity score model or the outcome model are correctly specified, as discussed further in Section 5. -- https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2943670/
* RA: can include propensity score as covariate?
* Bayesian bootstrap: https://stats.stackexchange.com/questions/181350/bootstrapping-vs-bayesian-bootstrapping-conceptually
* Full matching: special case of subclassification; each stratum has at least one treated & one control

## Diagnosing and responding to violations in the positivity assumption
https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4107929/pdf/nihms603976.pdf
* Restriction of the covariate adjustment set; alternative projection function; restriction of the sample; modification of the target intervention
* ~= data trimming + "treatment trimming" + "covariate trimming" + alternative projection function
* 2 reasons for positivity violation (PV): theoretically impossible; chance b/c finite sample size
* Sparsity > PV
* NPSEM: nonparametric structural equation model
* Marginal structural model: (3) and (4)
* Positivity using projection function: (8)
* 3 ways to estimate ATE: G-computation; IPW; double robust (G-computation + IPW)
* Curse of dimensionality -> cross-validated loss-based learning?!
* IPW: = IPTW; can think of it as weighting rare samples; weight truncation & stabilized weights (for sparsity)
* IPW estimator can't extrapolate based on Q (G-computation)?!; extrapolation seems possible just for propensity though
* Double robust estimator: A-IPTW & TMLE (= targeted MLE; G-computation -> propensity score -> update G-computation or its logit via perturbation)
* Diagnosing PV: informal description of strata; distribution of weights (inverse propensity); parametric bootstrap (sampling from our estimate a la bagging)
* Bootstrap: should ideally be applied to both IPW estimator and something else (to compare); yields optimistic estimator bias (thus red flag if large)
* PV red flag: bias estimate (via bootstrap) >= std error of the estimator(?); check bias-corrected confidence interval with the original
* Std error of estimator ~= std dev of estimator?
* Weight truncation => more bias / less variance?!
* > Diagnosis of substantial bias in the IPTW estimator due to positivity violations would have alerted an analyst that the G-computation estimator was relying heavily on extrapolation, and that the DR estimators were sensitive to bias arising from misspecification of the model used to estimate Ǭ0.
* Table 2 & 3 connected BUT Table 2 wouldn't be available in practice?!
* One solution to PV: change the projection function h(A, V) (= indicator function; = g(a | V) can lead to stabilized weights in the IPW model)
* Dangers of "trimming"
* Section 6.5: overall advice

## Practice of causal inference with the propensity of being zero or one: assessing the effect of arbitrary cutoffs of propensity scores
https://www.e-sciencecentral.org/upload/csam/pdf/csam-23-001.pdf
* PV may be hard to detect in practice b/c need to model propensity score s.t. near 0 or 1 pretty unlikely
* JCART = CART + jackknife resampling
* SUTVA: stable unit treatment value assumption
* RF: reduce variance
* 3 (+ 1) tree-based methods for estimating propensity score: CART; RF; GBM (generalized boosted model); + JCART
* Stop GBM when minimized ASAM (average standardized absolute mean difference)?
* GBM actually models logit(propensity): interesting way to do tree -> "logistic reg"?!
* GBM/boosting has the effect of restraining propensity score away from 0/1
* Jackknife > bootstrapping in terms of using more sample size per subsample
* 4 simulations
* Recommended ranges: [0.025, 0.975], [0.05, 0.95], [0.1, 0.9]
* Continuous multivariable (e.g. sum of two covariates) PV -> JCART's limitation?
* Paper not clear on: how GC; meaning of bias in Table 7-10; how JCART is used to predict ATE

## Invited Commentary: Positivity in Practice
https://academic.oup.com/aje/article-pdf/171/6/674/263877/kwp436.pdf
* Granular vs coarse categorization of covariates matters; tradeoff between bias due to PV & bias due to confounding (e.g. coarser age group)
* 2 solutions basically: restrict (change target population); interpolate/extrapolate

## Augmented Weighted Estimators Dealing with Practical Positivity Violation to Causal Inferences in a Random Coefficient Model
https://escholarship.org/content/qt21m8x39g/qt21m8x39g_noSplash_b5aaa3b89ddad94a26e52d82b5ac7e44.pdf
* Trimming -> handles PV but alters the causal estimand
* Intern-teaching (T = 1) vs student-teaching (T = 0) per school
* Also called AIPTW (but different "augmented"!)
* HLM: hierarchical linear model (aka multilevel model or linear mixed effect model)
* Two-level HLM via random coefficient model
* Equations (7), (8): very complicated; called AIPTW-HLM
* Simulation based on 4 models: AIPTW-SATC (school average T-corrected model), AIPTW-RSATC (reduced SATC), IPTW-orig, IPTW-trim
* Last section: (11) is much cleaner than (8)?!; vanilla version of AIPTW-HLM
* Main idea: include G-computation estimator when practical PV?
* Not sure how their simulations defined practical PV and reported numbers (e.g. 80% of schools had PV)

## A discriminative approach for finding and characterizing positivity violations using decision trees
https://arxiv.org/pdf/1907.08127.pdf
* Finding nonpositivity: hard with multivariate distributions
* > To overcome this generalization of marginals to joint overlap, [5] suggested tabular analysis... While their method characterizes the violating subspaces in a simple and intuitive way, it has two drawbacks: (1) it does not scale properly for more than a handful of covariates, due to its 2-dimensional tabular nature; and (2) the handling of continuous variables is suboptimal, as it is done by using predefined bins (e.g. quartiles). -- binning could be bad b/c sensitive to # bins
* > However, it also has two main drawbacks. Firstly, propensity scores can be viewed as a dimensionality reduction from the covariate space onto a single number, and almost always bear some information loss [7]. Specifically, two different subspaces can be mapped into the same scalar, suggesting an observed overlap can picture an overly optimistic view of the true violations in the original covariate space. Secondly, once a violation is detected, it is usually hard to characterize the covariate subspace causing it, as it depends on the interpretability of the model used to obtain those propensity scores. -- propensity score method drawbacks
* Their RF method: count fraction of trees with PV
* Their demos: diamond with one quadrant PV; t-SNE example (NHEFS dataset?) but bad example (b/c t-SNE shows no PV & their tree confirms it)
* https://www.hsph.harvard.edu/miguel-hernan/causal-inference-book/ -- NHEFS data
* Soft violation ~= practical PV

## Characterization of Overlap in Observational Studies
http://proceedings.mlr.press/v108/oberst20a/oberst20a.pdf
* > We formalize overlap estimation as a problem of finding minimum volume sets subject to coverage constraints and reduce this problem to binary classification with Boolean rule classifiers. -- abstract
* Off-policy: here relates to observational data from elsewhere?
* > In domain adaptation, the overlap between source and target domains is the set of inputs for which we can expect a trained model to transfer well (Ben-David et al., 2010; Johansson et al., 2019); In classification, overlap between inputs with different labels signifies regions that are hard to classify; In algorithmic fairness (Dwork et al., 2012), overlap between protected groups may shed light on disparate treatment of individuals from different groups who are otherwise comparable in task-relevant characteristics; In reinforcement learning, lack of overlap has been identified as a failure mode for deep Q-learning using experience replay (Fujimoto et al., 2019). -- relevance of overlap
* Reduction to 2 binary classification problems?
* > (D.1) They cover regions where all populations (treatment groups) are well-represented; (D.2) They exclude all other regions, including those outside the support of the study (see Figure 1); (D.3) They can be expressed using a small set of simple rules -- desiderata
* Support intersection (b/c non-informative) & propensity trimming (b/c Figure 1) not too good
* α-minimum-volume set S
* Overlap O (α, ε) = S (α) ∩ B (ε) with (S, B) = ("support", bounded propensity set)
* OverRule (algo to find overlap): ① find α-MV S (via Boolean rules) -> model propensity score -> ② find B restricted to S, i.e. B ∩ S (via Boolean rules)
* Boolean rules; binary classification (a la Neyman-Pearson); rule-based learning
* Remark: fitting S and B separately could increase interpretability?
* DNF: (Age < 30 and Female) or (Married); think of DNF ~= C (candidate set)
* ① Q(C): modified def of S with normalized volume (can estimate via uniform sampling for efficiency) + regularization per condition/clause; argmin Q(C) = S
* Binary classification (a la Neyman-Pearson): between p(X) (bad?!) and uniform distribution (good?!) over X
* Think of (normalized) volume ~= loss -> binary classification to minimize loss -> argmin
* Inspired by (2006): https://www.jmlr.org/papers/volume7/scott06a/scott06a.pdf
* ② slightly different but interestingly can be formulated as Neyman-Pearson
* ∃theorems bounding the generalization error for ① & ②
* MaxBox (2016): some other technique used as a base line to measure performance of OverRule
* Iris dataset; Jobs dataset; two real-world clinical datasets
* Other base overlap estimators?: CBB (covariate bounding box using marginal intersections); propensity score estimators; one-class SVM (OSVM)

## Propensity score adjustment using machine learning classification algorithms to control selection bias in online surveys
https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0231500
* PSA: propensity score adjustment
* Unclear if "adjustment" ~= reweighting
* Modeling propensity: interpretable (CART); black-box (neural net)
* Horvitz-Thompson weight vs Hajek weight -- check: https://imai.fas.harvard.edu/teaching/files/weighting.pdf & https://www.samsi.info/wp-content/uploads/2016/03/Ye-Yang_may6_2014.pdf
* ML alternatives to logistic reg: decision tree; RF; GBM; kNN; Naive Bayes
* kNN becomes less efficient with more dimensions
* Simulation study: 500+ simulations with Hajek weight -> bias/MSE to estimate "proportion of voters"
* Not really measuring anything causal (e.g. Y, ATE)?!; just propensity score -> reweighting -> guess global (everyone) parameters from local (online surveys)

## Causal Inference What If textbook (Chapters 11-12)
https://cdn1.sph.harvard.edu/wp-content/uploads/sites/1268/2020/11/ciwhatif_hernanrobins_23nov20.pdf
* OLS: just expectable linear regression
* Saturated model (e.g. linear reg for binary input/output): when # parameters = # outputs
* Hajek: IP weighted least squares (WLS) regression possible; doesn't estimate E[Y] with PV?!; need to take IP weighting into account for confidence interval
* Stabilized IP weighting: WLS also possible
* Marginal structural model (MSM): linear/logistic; similar to G-com (E[Y^a | V] = β0 + β1 a + β2 a V + β3 V) except no unconfoundedness assumption (i.e. a ~= do(a))
* Effect modification ~= confounding - unconfoundedness assumption of the model
* Technical Point 12.2: formula for stabilized weighting -> Y as per IPW

## Estimation of causal effects with multiple treatments: a review and new ideas
https://arxiv.org/pdf/1701.05132.pdf
* What happens if treatment is not binary?
* Generalized propensity score: vector
* CRM (common referent matching), IPW, KMC (k-means clustering somehow), VM (their idea: vector matching)

## Estimating causal effects for multivalued treatments: a comparison of approaches
http://lindenconsulting.org/documents/Multivalued_Article.pdf
* 3 approaches: regression adjustment (RA); weighted estimator (IPTW & MMWS = marginal mean weighting through stratification); doubly robust (A-IPTW & IPTW-RA)
* Outcome model vs treatment model (estimating propensity score)
* RA: separate (linear) regression per treatment (i.e. no t in the equation); critical are model misspecification & extrapolation
* IPTW: typical ATE (can use GPS)
* IPTW2: Hajek; emerges naturally from GMM (generalized method of moments) representation of treatment effects?
* MMWS = propensity score stratification (interesting!) + IPTW (+ stabilized weighting) -- Equation 10 shows stabilized weighting
* Equation 11: shows Hajek-style fraction (interesting!) to get Y via MMWS
* A-IPTW: Y (typical formula) = Cattaneo's efficient influence function estimator
* IPTW-RA: weighting -> use weighted regression -> ATE; doubly robust!
* GMM framework can derive A-IPTW & IPTW-RA in one step?
* A-IPTW & IPTW-RA performed best; paper supports doubly robust

## Use of Stabilized Inverse Propensity Scores as Weights to Directly Estimate Relative Risk and Its Confidence Intervals
https://www.sciencedirect.com/science/article/pii/S1098301510603725
* Shows N_{IPTW weighting} = 2 * N_{stabilized weighting} = 2 * N_{of sample}
* Binary output -> E[Y] = P(Y) -> weighted logistic regression (unclear) using IPTW vs SW (stabilized weighting)

## Estimation of Causal Effects of Multiple Treatments in Observational Studies with a Binary Outcome
https://arxiv.org/pdf/2001.06483.pdf
* Multiple treatments & binary outcome
* BART vs {IPTW, TMLE, vector matching, regression adjustment}
* Adding GBM to IPTW (IPTW-GBM) provides better bias reduction & smaller RMSE
* GBM: can use as alternative to logistic reg; better b/c alleviates extreme weights
* Motivation: non-small cell lung cancer (NSCLC)
* RA (aka model-based imputation)
* BART's advantages: extremely flexible functional form; doesn't suffer from bad propensity score model; coherent conf intervals > PS matching & subclassification; easy to get heterogeneous treatment effect as well; easy to implement / efficient (e.g. compared to IPTW-GBM)
* What does the paper say about BART <> extrapolation? A. Hard to know when extrapolating -> should know where overlap is via BART itself somehow (BART still suffers from extrapolation)
* Their BART <> overlap method: improvement of 1 sd rule by Hill and Su (in breastfeeding paper); 0 sd rule & generalize to multi-treatment setting
* Simulation 1: comparison of 10 models
* Simulation 2: comparison of IPTW-GBM (with/without trimming), BART (with/without discarding rules), TMLE -- which performed best in Simulation 1
* > Using BART, the percentages of discarded units in the treated group, averaged across 200 replications, in the weak, moderateI, moderateII and strong scenario were 38%, 24%, 15% and 0.2%, respectively, as compared to 86%, 42%, 42% and 13% computed by the GPS-based discarding rule -- BART's discarding rules discard the least!

## Covariate balancing propensity score
https://imai.fas.harvard.edu/research/files/CBPS.pdf
* CBPS: covariate balancing propensity score method
* Weights assigned s.t. optimize covariate balance
* Estimation done via generalized method of moments (GMM) or empirical likelihood (EL) frameworks
* > In this paper, we introduce the covariate balancing propensity score (CBPS) and show how to estimate the propensity score such that the resulting covariate balance is optimized. We exploit the dual characteristics of the propensity score as a covariate balancing score and the conditional probability of treatment assignment. Specifically, we use a set of moment conditions that are implied by the covariate balancing property (i.e. mean independence between the treatment and covariates after inverse propensity score weighting) to estimate the propensity score while also incorporating the standard estimation procedure (i.e. the score condition for the maximum likelihood) whenever appropriate. As will be shown, satisfying the score condition can be seen as a particular covariate balancing condition.
* Need to satisfy: covariate balancing conditions E{...} f(X) = 0
* CBPS1: when taking f(X) = X
* CBPS2: when taking f(X) = their score condition in (7)
* Actual estimation via GMM or EL

## Data-adaptive selection of the truncation level for Inverse-Probability-of-Treatment-Weighted estimators
https://core.ac.uk/download/pdf/61321376.pdf
* Data-adaptively truncated IPTW estimator (tIPTW in R): can select appropriate weight truncation levels data-adaptively
* Adds an extra "nuisance" estimator though it doesn't need to be a consistent one

## Effects of Adjusting for Instrumental Variables on Bias and Precision of Effect Estimates
https://pdfs.semanticscholar.org/4575/b7d629714ddc48b62dce3c86401c1dc3b9db.pdf
* Conditioning on IV, near-IV (weakly associated with outcome), confounders; the first two usually yield larger bias & variance?
* Minimizing unmeasured confounding should be priority (even at the risk of conditioning on IV)
* IV may look like confounder in practice
* Wright's rule of path analysis: no collider path; one fork path is fine -- https://arxiv.org/ftp/arxiv/papers/1203/1203.3503.pdf by Pearl
* Example study by Patrick: including prior glaucoma diagnosis to covariate (when predicting mortality / hip fracture) -> bigger error in estimator (b/c IV)
* The paper's "exposure" = treatment; "predictors of exposure" ~= IV
* Main point: conditioning on IV not good but not too bad

## Instrumental variables as bias amplifiers with general outcome and confounding
https://arxiv.org/pdf/1701.04177.pdf
* Bias amplifier: those (like IV) that increase bias when conditioned on
* "Conditioning on IV can increase bias" has only been for linear models -> paper tries to generalize (+ certain monotonicity assumptions)
* Point: need to be careful about "let's condition on as many things as possible"

## ASSESSING LACK OF COMMON SUPPORT IN CAUSAL INFERENCE USING BAYESIAN NONPARAMETRICS: IMPLICATIONS FOR EVALUATING THE EFFECT OF BREASTFEEDING ON CHILDREN’S COGNITIVE OUTCOMES
https://projecteuclid.org/download/pdfview_1/euclid.aoas/1380804800
* Distinction between common support and common causal support (only including covariates relevant in ignorability assumption), which BART deals more with
* Data from National Longitudinal Survey of Youth (breastfeeding -> math/reading achievement at age 5-6)
* > Comparisons of posterior distributions of counterfactual outcomes versus factual (observed) outcomes can be used to create red flags when the amount of information about the counterfactual outcome for a given observation is not sufficient to warrant making inferences about that observation.
* BART: suggestion of 200 trees (for sum-of-trees model); regularization prior (prevents overfitting); 2 types of draws? (for g and f if f = sum of g + ε); compute ATE (or others) by f^r (r-th draw of f in BART's iterations)
* 2 disadvantages to propensity/weighting: unstable; loses information and interpretation to a point of little practical relevance
* > The simple idea is to capitalize on the fact that the posterior standard deviations of the individual-level conditional expectations estimated using BART increase markedly in areas that lack common causal support -- monitor spike in standard deviation (see Figure 1)
* BART f shows information about f(t, X) -> is there enough information in f(1 - t, X)? -> std deviation (interval) would be big if not
* σ^{f_t} = sd(f(t, X)) where sd() is posterior standard deviation (for the interval)
* Sd ~= posterior sd in this context
* 1 sd rule: BART discarding rule if counterfactual sd > (max sd of observed_t) + (sd of sd of observed_t) -- page 8
* The above 1 sd rule needs Var(Y | T = 1, X) ~= Var(Y | T = 0, X) for maximum effect
* Modified (unclear better) 1 sd rule: cutoff based on (ratio of counterfactual sd to sd)^2; could be less stable / discard based on relative not absolute
* Practically: "flag" based on 1 sd rule -> use researcher knowledge
* Simulations: BART with 1 sd rule > matching (with discard) or IPTW (with discard)
* Interpretability of PV: use simple tree with (input, output) = (X, counterfactual sd - max sd of observed_t - sd of sd of observed_t) on PV set
* Worthwhile: discard by BART vs discard by propensity

## A Practical Introduction to Bayesian Estimation of Causal Effects: Parametric and Nonparametric Approaches
https://arxiv.org/pdf/2004.07375.pdf
* > we demonstrate how priors can induce shrinkage and sparsity in parametric models -- shrinkage ~= regularization; sparsity: few nonzero parameters
* Time-varying treatment settings also considered
* > First, Bayesian modeling yields full posterior inference for any function of model parameters. For instance, point and interval estimates can be easily constructed for causal risk ratios, odds ratios, and risk differences by post-processing a single set of posterior draws from logistic regression model. Another advantage is the use of priors to induce shrinkage and sparsity in causal models - yielding more regularized causal effect estimates. We show that these can be more satisfying than the ad-hoc alternatives often employed. Priors can also be used to conduct probabilistic sensitivity analyses around violations of key causal identification assumptions. Finally, the Bayesian literature consists of a large suite of nonparametric models that can be readily applied to causal modeling. These nonparametric approaches are appealing because, unlike many classical machine learning algorithms, they allow for posterior uncertainty estimation as well as robust point estimates -- Bayes. pros
* Partially pool conditional ATE? Bayesian bootstrap?
* 3 algos: Dirichlet process priors (Dirichlet process mixture model); BART; Gaussian process priors (Gaussian process model)
* Section 2.3: basic concepts behind Bayesian causal inference (though not too clear to me)
* Idea: priors can be specified in a principled way! (e.g. for regularization, etc.)
* Section 3.3: Bayesian bootstrap (more detailed)
* Section 4: Bayesian approach can help with time-varying treatment (DAG Figure 2)
* > The Bayesian literature has explored several such “sparsity” priors, including the horseshoe, LASSO, and spike-and-slab priors
* It seems there exists Bayesian parametric (how does this work?) vs nonparametric distinction

## Estimating Population Average Causal Effects in the Presence of Non-Overlap: The Effect of Natural Gas Compressor Station Exposure on Cancer Mortality
https://arxiv.org/pdf/1805.09736.pdf
* 2 contributions: 1. data-driven definition of propensity score overlap; 2. novel Bayesian framework for extrapolating
* > causal effects of compressor stations on county-level thyroid cancer and leukemia mortality rates
* RO (region of overlap), RN (non-overlap): two separate models
* They use unconfoundedness (instead of ignorability) & positivity as 2 key assumptions
* Tradeoff between unconfoundedness (with more covariates) & positivity: interesting to note
* Modifying estimand could be ok in some context / less ideal (e.g. policymaking) in another
* Goals when extrapolating: small bias if possible (of course); minimize model dependence
* BART+SPL: RO (estimate via BART) -> ⍟ RN (estimate via SPL = spline method)
* #1. new definition of overlap: more than b units exist within open ball (with radius a in terms of propensity) around point x
* Restricted cubic spline (RCS); restricted ~= natural spline; smoothing spline (regularized regression over natural spline basis)
* ⍟ SPL: RCS (one on propensity score + one on Y); estimate involves tuning noise parameter \tau (proportional to |observed e() - nearest e() in RO|) -> increases variance with uncertainty -> still not too bad of an interval
* Iterate (BART -> SPL) M times -> average!
* Can use Bayesian bootstrap -> population ATE?

## Balancing Covariates via Propensity Score Weighting
http://www2.stat.duke.edu/~fl35/papers/psweight_final.pdf
* Balancing weights: generalization of most weights ~= my equation #3
* Overlap weights: (w_1, w_0) = (1 - e(), e())

## Discrete Optimization for Interpretable Study Populations and Randomization Inference in an Observational Study of Severe Sepsis Mortality
https://arxiv.org/pdf/1411.4873.pdf
* 2 contributions: maximal box problem; randomization inference to form conf interval for ATE with binary outcome
* 2 types of overlap: strong overlap; interpolation overlap
* Existing methods for achieving overlap: Crump (2009) / propensity score trimming; dealing with covariates themselves when defining experiments; BART's overlap
* Maximal box (pretty simple): find maximal box containing X+ but excluding any X-; 2 decision rules (page 19) for X+: 1. Dehejia & Wahba; 2. Crump
* NP-hard problem in general but poly time in fixed dimension p?!
* MaxBox extension possible (allowing C number of X-)
* Good to know which covariates matter when optimizing