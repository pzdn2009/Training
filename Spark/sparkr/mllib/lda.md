# LDA 

```r
# Load training data
df <- read.df("data/mllib/sample_lda_libsvm_data.txt", source = "libsvm")
training <- df
test <- df

# Fit a latent dirichlet allocation model with spark.lda
model <- spark.lda(training, k = 10, maxIter = 10)

# Model summary
summary(model)

# Posterior probabilities
posterior <- spark.posterior(model, test)
showDF(posterior)

# The log perplexity of the LDA model
logPerplexity <- spark.perplexity(model, test)
print(paste0("The upper bound bound on perplexity: ", logPerplexity))

```