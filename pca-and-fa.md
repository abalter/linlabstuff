https://stats.stackexchange.com/questions/1576/what-are-the-differences-between-factor-analysis-and-principal-component-analysi?noredirect=1&lq=1

**Principal component analysis** involves extracting linear composites of observed variables.

**Factor analysis** is based on a formal model predicting observed variables from theoretical latent factors.

In psychology these two techniques are often applied in the construction of multi-scale tests
 to determine which items load on which scales.
They typically yield similar substantive conclusions (for a discussion see Comrey (1988) Factor-Analytic Methods of Scale Development in Personality and Clinical Psychology).
This helps to explain why some statistics packages seem to bundle them together.
I have also seen situations where "principal component analysis" is incorrectly labelled "factor analysis".

In terms of a **simple rule of thumb**, I'd suggest that you:

1. Run factor analysis if you  assume or wish to test a theoretical model of latent factors causing observed variables.

2. Run principal component analysis If you want to simply reduce your correlated observed variables to a smaller set of important independent composite variables.




http://alumni.media.mit.edu/~tpminka/statlearn/glossary/glossary.html


https://stats.stackexchange.com/a/133806/141304

https://stats.stackexchange.com/a/288646/141304

This answer is to show concrete computational similarities and differences between PCA and Factor analysis. For general theoretical differences between them, see questions/answers [1][1], [2][2], [3][3], [4][4], [5][5].

Below I will do, step by step, **Principal Component analysis (PCA)** of [_iris data_][6] ("setosa" species only)  and then will do Factor analysis of the same data. **Factor analysis (FA)** will be done by [Iterative principal axis][7] (**PAF**) method which is based on PCA approach and thus makes one able to compare PCA and FA step-by-step.

Iris data (setosa only):

      id  SLength   SWidth  PLength   PWidth species 
 
       1      5.1      3.5      1.4       .2 setosa 
       2      4.9      3.0      1.4       .2 setosa 
       3      4.7      3.2      1.3       .2 setosa 
       4      4.6      3.1      1.5       .2 setosa 
       5      5.0      3.6      1.4       .2 setosa 
       6      5.4      3.9      1.7       .4 setosa 
       7      4.6      3.4      1.4       .3 setosa 
       8      5.0      3.4      1.5       .2 setosa 
       9      4.4      2.9      1.4       .2 setosa 
      10      4.9      3.1      1.5       .1 setosa 
      11      5.4      3.7      1.5       .2 setosa 
      12      4.8      3.4      1.6       .2 setosa 
      13      4.8      3.0      1.4       .1 setosa 
      14      4.3      3.0      1.1       .1 setosa 
      15      5.8      4.0      1.2       .2 setosa 
      16      5.7      4.4      1.5       .4 setosa 
      17      5.4      3.9      1.3       .4 setosa 
      18      5.1      3.5      1.4       .3 setosa 
      19      5.7      3.8      1.7       .3 setosa 
      20      5.1      3.8      1.5       .3 setosa 
      21      5.4      3.4      1.7       .2 setosa 
      22      5.1      3.7      1.5       .4 setosa 
      23      4.6      3.6      1.0       .2 setosa 
      24      5.1      3.3      1.7       .5 setosa 
      25      4.8      3.4      1.9       .2 setosa 
      26      5.0      3.0      1.6       .2 setosa 
      27      5.0      3.4      1.6       .4 setosa 
      28      5.2      3.5      1.5       .2 setosa 
      29      5.2      3.4      1.4       .2 setosa 
      30      4.7      3.2      1.6       .2 setosa 
      31      4.8      3.1      1.6       .2 setosa 
      32      5.4      3.4      1.5       .4 setosa 
      33      5.2      4.1      1.5       .1 setosa 
      34      5.5      4.2      1.4       .2 setosa 
      35      4.9      3.1      1.5       .2 setosa 
      36      5.0      3.2      1.2       .2 setosa 
      37      5.5      3.5      1.3       .2 setosa 
      38      4.9      3.6      1.4       .1 setosa 
      39      4.4      3.0      1.3       .2 setosa 
      40      5.1      3.4      1.5       .2 setosa 
      41      5.0      3.5      1.3       .3 setosa 
      42      4.5      2.3      1.3       .3 setosa 
      43      4.4      3.2      1.3       .2 setosa 
      44      5.0      3.5      1.6       .6 setosa 
      45      5.1      3.8      1.9       .4 setosa 
      46      4.8      3.0      1.4       .3 setosa 
      47      5.1      3.8      1.6       .2 setosa 
      48      4.6      3.2      1.4       .2 setosa 
      49      5.3      3.7      1.5       .2 setosa 
      50      5.0      3.3      1.4       .2 setosa 

We have 4 numeric variables to include in our analyses: *SLength SWidth PLength PWidth*, and the analyses will be based on _covariances_, which is the same as to say that we analyse _centered_ variables. (If we chose to analyse correlations that would be analysing standardized variables. Analysis based on correlations [produce different results][8] than analysis based on covariances.) I will not display the centered data. Let's call these data matrix `X`.

**PCA** steps:

    Step 0. Compute centered variables X and covariance matrix S.

    Covariances S (= X'*X/(n-1) matrix: see https://stats.stackexchange.com/a/22520/3277)
    .12424898	.09921633	.01635510	.01033061
    .09921633	.14368980	.01169796	.00929796
    .01635510	.01169796	.03015918	.00606939
    .01033061	.00929796	.00606939	.01110612

    Step 1.1. Decompose data X or matrix S to get eigenvalues and right eigenvectors.
              You may use svd or eigen decomposition (see https://stats.stackexchange.com/q/79043/3277)

    Eigenvalues L (component variances) and the proportion of overall variance explained
               L            Prop
    PC1   .2364556901   .7647237023 
    PC2   .0369187324   .1193992401 
    PC3   .0267963986   .0866624997 
    PC4   .0090332606   .0292145579    

    Eigenvectors V (cosines of rotation of variables into components)
                  PC1           PC2           PC3           PC4
    SLength   .6690784044   .5978840102  -.4399627716  -.0360771206 
    SWidth    .7341478283  -.6206734170   .2746074698  -.0195502716 
    PLength   .0965438987   .4900555922   .8324494972  -.2399012853 
    PWidth    .0635635941   .1309379098   .1950675055   .9699296890 

    Step 1.2. Decide on the number M of first PCs you want to retain.
              You may decide it now or later on - no difference, because in PCA values of components do not depend on M.
              Let's M=2. So, leave only 2 first eigenvalues and 2 first eigenvector columns.

    Step 2. Compute loadings A. May skip if you don't need to interpret PCs anyhow.
    Loadings are eigenvectors normalized to respective eigenvalues: A value = V value * sqrt(L value)
    Loadings are the covariances between variables and components.

    Loadings A
                  PC1           PC2           
    SLength    .32535081     .11487892
    SWidth     .35699193    -.11925773
    PLength    .04694612     .09416050
    PWidth     .03090888     .02515873

    Sums of squares in columns of A are components' variances, the eigenvalues

    Standardized (rescaled) loadings.
    St. loading is Loading / sqrt(Variable's variance);
    these loadings are computed if you analyse covariances, and are suitable for interpretation of PCs
    (if you analyse correlations, A are already standardized).
                  PC1           PC2      
    SLength    .92300804     .32590717
    SWidth     .94177127    -.31461076
    PLength    .27032731     .54219930
    PWidth     .29329327     .23873031

    Step 3. Compute component scores (values of PCs).

    Regression coefficients B to compute Standardized component scores are: B = A*diag(1/L) = inv(S)*A
    B
                  PC1           PC2  
    SLength   1.375948338   3.111670112 
    SWidth    1.509762499  -3.230276923 
    PLength    .198540883   2.550480216 
    PWidth     .130717448    .681462580 

    Standardized component scores (having variances 1) = X*B
          PC1           PC2
      .219719506   -.129560000 
     -.810351411    .863244439 
     -.803442667   -.660192989 
    -1.052305574   -.138236265 
      .233100923   -.763754703 
     1.322114762    .413266845 
     -.606159168  -1.294221106 
     -.048997489    .137348703 
      ...

    Raw component scores (having variances = eigenvalues) can of course be computed from standardized ones.
    In PCA, they are also computed directly as X*V
          PC1           PC2
      .106842367   -.024893980 
     -.394047228    .165865927 
     -.390687734   -.126851118 
     -.511701577   -.026561059 
      .113349309   -.146749722 
      .642900908    .079406116 
     -.294755259   -.248674852 
     -.023825867    .026390520 
      ...
**FA** (iterative principal axis extraction method) steps:

    Step 0.1. Compute centered variables X and covariance matrix S.

    Step 0.2. Decide on the number of factors M to extract.
              (There exist several well-known methods in help to decide, let's omit mentioning them. Most of them require that you do PCA first.)
              Note that you have to select M before you proceed further because, unlike in PCA, in FA loadings and factor values depend on M.
              Let's M=2.

    Step 0.3. Set initial communalities on the diagonal of S.
              Most often quantities called "images" are used as initial communalities (see https://stats.stackexchange.com/a/43224/3277).
              Images are diagonal elements of matrix S-D, where D is diagonal matrix with diagonal = 1 / diagonal of inv(S).
              (If S is correlation matrix, images are the squared multiple correlation coefficients.)

    With covariance matrix, image is the squared multiple correlation multiplied by the variable variance.
    S with images as initial communalities on the diagonal
    .07146025  .09921633  .01635510  .01033061
    .09921633  .07946595  .01169796  .00929796
    .01635510  .01169796  .00437017  .00606939
    .01033061  .00929796  .00606939  .00167624

    Step 1. Decompose that modified S to get eigenvalues and right eigenvectors.
            Use eigen decomposition, not svd. (Usually some last eigenvalues will be negative.)

    Eigenvalues L
    F1   .1782099114
    F2   .0062074477
        -.0030958623
        -.0243488794
 
    Eigenvectors V
                   F1            F2 
    SLength   .6875564132   .0145988554   .0466389510   .7244845480
    SWidth    .7122191394   .1808121121  -.0560070806  -.6759542030
    PLength   .1154657746  -.7640573143   .6203992617  -.1341224497
    PWidth    .0817173855  -.6191205651  -.7808922917  -.0148062006

    Leave the first M=2 values in L and columns in V.

    Step 2.1. Compute loadings A.
    Loadings are eigenvectors normalized to respective eigenvalues: A value = V value * sqrt(L value)
                   F1            F2 
    SLength   .2902513607   .0011502052
    SWidth    .3006627098   .0142457085
    PLength   .0487437795  -.0601980567
    PWidth    .0344969255  -.0487788732

    Step 2.2. Compute row sums of squared loadings. These are updated communalities.
              Reset the diagonal of S to them

    S with updated communalities on the diagonal
    .08424718  .09921633  .01635510  .01033061
    .09921633  .09060101  .01169796  .00929796
    .01635510  .01169796  .00599976  .00606939
    .01033061  .00929796  .00606939  .00356942

    REPEAT Steps 1-2 many times (iterations, say, 25)

    Extraction of factors is done.

    Final loadings A and communalities (row sums of squares in A).
    Loadings are the covariances between variables and factors.
    Communality is the degree to what the factors load a variable, it is the "common variance" in the variable.
                   F1            F2                        Comm
    SLength   .3125767362   .0128306509                .0978688416
    SWidth    .3187577564  -.0323523347                .1026531808
    PLength   .0476237419   .1034495601                .0129698323
    PWidth    .0324478281   .0423861795                .0028494498

    Sums of squares in columns of A are factors' variances.

    Standardized (rescaled) loadings and communalities.
    St. loading is Loading / sqrt(Variable's variance);
    these loadings are computed if you analyse covariances, and are suitable for interpretation of Fs
    (if you analyse correlations, A are already standardized).
                   F1            F2                        Comm
    SLength   .8867684574   .0364000747                .7876832626
    SWidth    .8409066701  -.0853478652                .7144082859
    PLength   .2742292179   .5956880078                .4300458666
    PWidth    .3078962532   .4022009053                .2565656710

    Step 3. Compute factor scores (values of Fs).
            Unlike component scores in PCA, factor scores are not exact, they are reasonable approximations.
            Several methods of computation exist (https://stats.stackexchange.com/q/126885/3277).
            Here is regressional method which is the same as the one used in PCA.

    Regression coefficients B to compute Standardized factor scores are: B = inv(S)*A (original S is used)
    B
                  F1           F2  
    SLength  1.597852081   -.023604439
    SWidth   1.070410719   -.637149341
    PLength   .212220217   3.157497050
    PWidth    .423222047   2.646300951

    Standardized factor scores = X*B
    These "Standardized factor scores" have variance not 1; the variance of a factor is SSregression of the factor by variables / (n-1).
          F1           F2
      .194641800   -.365588231
     -.660133976   -.042292672
     -.786844270   -.480751358
    -1.011226507    .216823430
      .141897664   -.426942721
     1.250472186    .848980006
     -.669003108   -.025440982
     -.050962459    .016236852
      ...

    Factors are extracted as orthogonal. And they are.
    However, regressionally computed factor scores are not fully uncorrelated.
    Covariance matrix between computed factor scores.
          F1      F2
    F1   .864   .026
    F2   .026   .459

    Factor variances are their squared loadings.
    You can easily recompute the above "standardized" factor scores to "raw" factor scores having those variances:
    raw score = st. score * sqrt(factor variance / st. scores variance).

After the extraction (shown above), optional rotation may take place. Rotation is frequently done in FA. Sometimes it is done in PCA exactly the same way. Rotation rotates loading matrix **A** into some form of "simple structure" which facilitates interpretation of factors greatly (then rotated scores can be recomputed). Since rotation is not what differentiates FA from PCA mathematically and because it is a separate large topic, I won't touch it.


  [1]: https://stats.stackexchange.com/q/1576/3277
  [2]: https://stats.stackexchange.com/a/849/3277
  [3]: https://stats.stackexchange.com/q/94048/3277
  [4]: https://stats.stackexchange.com/a/95106/3277
  [5]: https://stats.stackexchange.com/a/4760/3277
  [6]: http://en.wikipedia.org/wiki/Iris_flower_data_set
  [7]: https://stats.stackexchange.com/q/50745/3277
  [8]: https://stats.stackexchange.com/q/62677/3277
  


https://stats.stackexchange.com/questions/123063/is-there-any-good-reason-to-use-pca-instead-of-efa-also-can-pca-be-a-substitut

https://stats.stackexchange.com/questions/1576/what-are-the-differences-between-factor-analysis-and-principal-component-analysi/133806#133806

http://localhost:8787/ | RStudio - MiSeq_239
http://localhost:6789/lab | JupyterLab
https://stackoverflow.com/questions/20295787/how-can-i-use-the-row-names-attribute-to-order-the-rows-of-my-dataframe-in-r | How can I use the row.names attribute to order the rows of my dataframe in R? - Stack Overflow
https://stackoverflow.com/questions/36410485/how-to-order-a-data-frame-based-on-row-names-in-another-data-frame?rq=1 | r - How to order a data.frame based on row.names in another data frame? - Stack Overflow
https://stackoverflow.com/questions/10202480/aggregate-rows-by-shared-variables | r - Aggregate rows by shared variables - Stack Overflow
https://stats.stackexchange.com/questions/169056/aggregate-all-data-by-date-and-id | r - aggregate all data by Date and ID - Cross Validated
https://www.r-statistics.com/2012/01/aggregation-and-restructuring-data-from-r-in-action/ | Aggregation and Restructuring data (from "R in Action") | R-statistics blog
https://github.com/abalter/microbiome-16s/new/master | New File
https://stats.stackexchange.com/questions/1576/what-are-the-differences-between-factor-analysis-and-principal-component-analysi/133806#133806 | pca - What are the differences between Factor Analysis and Principal Component Analysis? - Cross Validated
https://stats.stackexchange.com/questions/127483/where-is-the-indeterminacy-of-factor-values-on-this-plot-explaining-factor-analy | pca - Where is the indeterminacy of factor values on this plot explaining factor analysis? - Cross Validated
https://stats.stackexchange.com/questions/279062/are-principal-components-reflective-formative-both-or-neither | pca - Are principal components reflective, formative, both, or neither? - Cross Validated
https://mathoverflow.net/questions/40191/the-difference-between-principal-components-analysis-pca-and-factor-analysis | eigenvector - The difference between Principal Components Analysis (PCA) and Factor Analysis (FA) - MathOverflow
https://stats.stackexchange.com/questions/1576/what-are-the-differences-between-factor-analysis-and-principal-component-analysis | pca - What are the differences between Factor Analysis and Principal Component Analysis? - Cross Validated
https://stats.stackexchange.com/questions/143905/loadings-vs-eigenvectors-in-pca-when-to-use-one-or-another?noredirect=1&lq=1 | Loadings vs eigenvectors in PCA: when to use one or another? - Cross Validated
https://stats.stackexchange.com/questions/95038/how-does-factor-analysis-explain-the-covariance-while-pca-explains-the-variance?noredirect=1&lq=1 | How does Factor Analysis explain the covariance while PCA explains the variance? - Cross Validated
https://stats.stackexchange.com/questions/62677/pca-on-correlation-or-covariance-does-pca-on-correlation-ever-make-sense?noredirect=1&lq=1 | factor analysis - PCA on correlation or covariance: does PCA on correlation ever make sense? - Cross Validated
https://stats.stackexchange.com/questions/79043/why-pca-of-data-by-means-of-svd-of-the-data?noredirect=1&lq=1 | algorithms - Why PCA of data by means of SVD of the data? - Cross Validated
https://stats.stackexchange.com/questions/94048/pca-and-exploratory-factor-analysis-on-the-same-dataset-differences-and-similar?noredirect=1&lq=1 | PCA and exploratory Factor Analysis on the same dataset: differences and similarities; factor model vs PCA - Cross Validated
https://stats.stackexchange.com/questions/17090/pca-and-fa-example-calculation-of-communalities | factor analysis - PCA and FA example - calculation of communalities - Cross Validated
https://stats.stackexchange.com/questions/4734/what-is-the-difference-between-exploratory-factor-analysis-and-principal-compone | What is the difference between Exploratory Factor Analysis and Principal Components Analysis (PCA)? - Cross Validated
https://stats.stackexchange.com/questions/220654/toy-example-dataset-for-testing-pca-implementation | Toy example dataset for testing PCA implementation - Cross Validated
https://stats.stackexchange.com/questions/152283/confused-about-scores-and-loadings-in-this-pca-biplot | Confused about scores and loadings in this PCA biplot - Cross Validated
https://stats.stackexchange.com/questions/241726/understanding-exploratory-factor-analysis-some-points-for-clarification | clustering - Understanding (exploratory) factor analysis: some points for clarification - Cross Validated
https://stats.stackexchange.com/questions/161417/pca-dataset-to-test-my-implementation | PCA dataset to test my implementation - Cross Validated
mailto:?body=Paste%20copied%20links%20here | 
