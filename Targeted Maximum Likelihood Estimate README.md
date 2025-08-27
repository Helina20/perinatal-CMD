Targeted Maximum Likelihood Estimation (TMLE) with Super Learner in RThis repository contains an example implementation of Targeted Maximum Likelihood Estimation (TMLE) using the tmle and SuperLearner packages in R.The workflow demonstrates how to estimate the Average Treatment Effect (ATE) while adjusting for confounders with machine learning–based nuisance parameter estimation.📦 RequirementsR (version >= 4.0.0 recommended)The following R packages (run install.packages() once):tmleSuperLearnerglmnetrangergam🚀 Key Steps in the ScriptThe main script follows these steps:Load required libraries:library(tmle)
library(SuperLearner)
Define variables:Y: Binary outcome (e.g., health outcome)A: Binary exposure/treatmentW: Covariates (confounders)Define Super Learner libraries:For the outcome regression (Q): SL.mean, SL.glm, SL.glmnet, SL.rangerFor the propensity score (g): SL.glm, SL.glmnet, SL.rangerRun TMLE:fit_tmle <- tmle(
  Y = Y,
  A = A,
  W = W,
  family = "binomial",
  Q.SL.library = SL_lib_Q,
  g.SL.library = SL_lib_g,
  gbound = c(0.01, 0.99),
  verbose = TRUE
)
Summarize results:ate_hat <- fit_tmle$estimates$ATE$psi
ci_lower <- fit_tmle$estimates$ATE$CI[1]
ci_upper <- fit_tmle$estimates$ATE$CI[2]
p_value <- fit_tmle$estimates$ATE$pvalue

cat(sprintf("\nATE (TMLE): %.3f  95%% CI [%.3f, %.3f],  p = %.4f\n",
            ate_hat, ci_lower, ci_upper, p_value))
📖 NotesReplace Y, A, and W with the variables from your dataset.Ensure that both A and Y are binary.The gbound parameter, set at c(0.01, 0.99), is used to prevent instability in the TMLE estimate caused by extreme propensity scores.