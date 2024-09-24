# CLA Example Gallery

Markowitz' Critical Line Algorithm (CLA) explores the Pareto frontier of the bi-objective optimization problem

$$
    \min \mathbf{w}^T\boldsymbol{\mu},\ \max \mathbf{w}^T\Sigma \mathbf{w}\ \text{ subject to }\ \mathbf{w}^T\mathbf{e} = 1,\ \mathbf{l}\leq\mathbf{w}\leq\mathbf{u},
$$

with many problems in modern portfolio theory (MPT) building from here [1]. When developing algorithms and software to solve these, it's important to have realistic test problems to check against.

However, many papers which discuss MPT either do not include their test data or reference datasets which are no longer available. To save others the time, here's a collation of relevant datasets which I compiled over the course of my PhD on CLA.

## Included Examples

Dataset Name       | Assets | Description                              | License     | Sources
:----------------- | -----: | :--------------------------------------- | :---------: | :------
[`StockIndex`]     |      6 | Monthly prices of stock indices          | [GPL v3+]   | [3]
[`StockIndexAdj`]  |      6 | Adjusted monthly prices of stock indices | [GPL v3+]   | [3]
[`StockIndexAdjD`] |      6 | Adjusted daily prices of stock indices   | [GPL v3+]   | [3]
[`ESCBFX`]         |      7 | Currency rates against the [EUR]         | [GPL v3+]   | [3]
[`Markowitz`]      |     10 | Markowitz' original ten asset problem    | [Unknown]   | [1]
[`DowJones`]       |     28 | [DJIA] asset prices                      | [CC-BY 4.0] | [2]
[`MultiAsset`]     |     10 | Stock and bond indices against [gold]    | [GPL v3+]   | [3]
[`INDTRACK1`]      |     31 | [HSI] asset prices                       | [MIT]       | [5] [6]
[`EuroStoxx50`]    |     48 | [EURO STOXX 50] asset prices             | [Unknown]   | [4]
[`FF49Industries`] |     49 | [Fama and French] 49 Industry prices     | [CC-BY 4.0] | [2] [7]
[`NASDAQ100`]      |     82 | [NASDAQ 100] asset prices                | [CC-BY 4.0] | [2]
[`FTSE100`]        |     83 | [FTSE 100] asset prices                  | [CC-BY 4.0] | [2]
[`INDTRACK2`]      |     85 | [DAX 100] asset prices                   | [MIT]       | [5] [6]
[`INDTRACK3`]      |     89 | [FTSE 100] asset prices                  | [MIT]       | [5] [6]
[`INDTRACK4`]      |     98 | [S&P 100] asset prices                   | [MIT]       | [5] [6]
[`INDTRACK5`]      |    225 | [Nikkei 225] asset prices                | [MIT]       | [5] [6]
[`MIBTEL`]         |    226 | [Borsa Italiana] asset prices            | [Unknown]   | [4]
[`SP500`]          |    442 | [S&P 500] asset prices                   | [CC-BY 4.0] | [2]
[`INDTRACK6`]      |    457 | [S&P 500] asset prices                   | [MIT]       | [6]
[`NASDAQComp`]     |   1203 | [NASDAQ Composite] asset prices          | [CC-BY 4.0] | [2]

## File Formats

These datasets have been standardised to use a consistent, plaintext file format so that they can more easily be used across multiple programming languages. The number of decimal places included in each dataset varies based on its original source.

### Time Series

Where available, datasets include the original asset price time series in a `timeseries.csv` file. Each row is a different time step, with the first column containing the date of the price (in YYYY-MM-DD format) if known or otherwise `T{i}`, where `{i}` is the step number. Each column is a different asset, with the first row containing the asset's name if known or otherwise `S{j}`, where `{j}` is the asset number. For example, the time series for [EuroStoxx50](Datasets/EuroStoxx50/) begins

```csv
EuroStoxx50, AABA.AS, ACA.PA, AGN.AS, AI.PA
2003-03-03,  10.4,    11.42,  5.54,   23.3  
2003-03-10,  11.26,   11.93,  6.58,   24.3  
2003-03-17,  12.16,   12.04,  6.7,    26.33 
2003-03-24,  11.34,   12.21,  6.17,   25.53 
```

Note that those datasets related to index tracking research begin with a column for the price of the index itself, followed by columns for a number of constituent assets. For example, the time series for [`INDTRACK1`] which tracks [HSI] begins

```csv
INDTRACK1, Index,         S1,         S2,          S3
T1,        8749.31759356, 9.33675195, 14.37788798, 7.36644878
T2,        8713.53262793, 9.86926631, 14.18263271, 7.40194974
T3,        9001.60515134, 9.95801871, 14.59089372, 7.77470979
T4,        8903.30299873, 9.15924716, 15.74467486, 7.63270596
```

### Mean Returns

Every dataset includes a `return.csv` file encoding expected returns for each asset. Where one was included with the original dataset, this has been retained. Otherwise, the mean is computed over 50 most recent time steps. The file has one asset per row, with the first column being the mean return and the second column being the standard deviation of the return. For example, the mean returns file for [INDTRACK1](Datasets/INDTRACK1/) begins

```csv
0.001309,0.043208
0.004177,0.040258
0.001487,0.041342
0.004515,0.044896
0.010865,0.069105
```

### Correlations

Every dataset includes a `risk.csv` file encoding the covariances between each pair of assets. Where one was included with the original dataset, this has been retained. Otherwise, the covariance matrix is computed over 50 most recent time steps. The covariance matrix is stored in [sparse matrix coordinate format](https://en.wikipedia.org/wiki/Sparse_matrix#Coordinate_list_(COO)) (COO), where for each non-zero matrix entry the first column gives its row index (from 1), the second column its column index, and the third its matrix value. For example, the risk file for [`INDTRACK1`] begins

```csv
0.0108650000,0.0047755010
0.0108609579,0.0047677406
0.0108569167,0.0047599943
0.0108528746,0.0047522585
0.0108488325,0.0047445351
```

### Full Problem

Some datasets (though not all) include a `problem.csv` file encoding all the main problem variables. The structure of the datafile is

```csv
S1,   S2,   ... , Sn
mu_1, mu_2, ... , mu_n
lb_1, lb_2, ... , lb_n
ub_1, ub_2, ... , ub_n
C_11, C_12, ... , C_1n
C_21, C_22, ... , C_2n
 .. ,  .. , ... ,  ..
C_n1, C_n2, ... , C_nn
```

where `mu` is the vector of respected returns, `lb` is the vector of lower weight bounds, `ub` is the vector of upper weight bounds, `and C_ij` is the (i, j)th entry of the covariance matrix. The first row gives identifying labels for each asset. For example, [`Xu3`] begins

### Efficient Frontiers

Some datasets (though not all) include a `frontier.csv` file encoding the unconstrained efficient frontiers. Each row is a calculated point on the unconstrained frontier, with the first column being the mean return and the second being the variance of return. This file contains no headers or labels. For example, the efficient frontier for [`INDTRACK1`] begins

```csv
1,1,1.000000
1,2,0.562289
1,3,0.746125
1,4,0.707857
1,5,0.336386
```

which corresponds to the first few entries in a matrix

$$
\Sigma = \begin{bmatrix}
    1.000000 & 0.562289 & 0.746125 & 0.707857 & 0.336386 & \ldots \\
    \vdots   & \vdots   & \vdots   & \vdots   & \vdots   & \ddots
\end{bmatrix}
$$

## Licenses

Be aware that this work is derived from datasets which were released under a range of different licenses. If a license above is given as **Unknown**, that means though the data was released publicly the author(s) did not specify the terms of its release; if used as part of further work it is at your own risk. In that and all cases, usage should be accompanied with a citation to the relevant source.

## Contributing

If you notice any errors in the included files, can provide an update to an existing dataset, or find an additional dataset that would be worth including, feel free to [get in touch](mailto:j.fogg@ed.ac.uk) or [open an issue on GitHub](https://github.com/Foggalong/cla-gallery/issues/new).

## Sources

1. Harry M. Markowitz (1987), _Mean-Variance Analysis in Portfolio Choice and Capital Markets_, B. Blackwell. ISBN: 0-631-15381-0.

2. Renato Bruni, Francesco Cesarone, Andrea Scozzari, and Fabio Tardella (September 2016), _Real-world datasets for portfolio selection and solutions of some stochastic dominance portfolio models_ in Data in Brief, Volume 8, Pages 858-862. ISSN: 2352-3409. DOI: [10.1016/j.dib.2016.06.031]. Data was constructed using a combination of information from _Thomson Reuters Datastream_ and the _Fama & French Data Library_, then checked to correct missing or inaccurate values.

3. Bernhard Pfaff (December 2012), _FRAPO: Financial Risk Modelling and Portfolio Optimisation with R_ in CRAN (Comprehensive R Archive Network), version 0.4. DOI: [10.32614/CRAN.package.FRAPO]. The FRAPO package [3] also includes dataset called `FTSE100`, `NASDAQ`, and `SP500`. These are all drawn from [4] and are (presumably) outdated versions of what are included in [2], so have been omitted here.

4. Francesco Cesarone, Andrea Scozzari, and Fabio Tardella (July 2015), _Linear vs. quadratic portfolio selection models with hard real-world constraints_ in Computer Management Science, Volume 12, Pages 345-370. DOI: [10.1007/s10287-014-0210-1]. This paper, via reference to [another], mentions additional datasets from [S&P 500] with 476 assets and one from [NASDAQ][NASDAQ Composite] which includes 2196 assets. However, these have been lost due to link rot.

5. T.-J. Chang, Nigel Meade, John E. Beasley, and Yazid M. Sharaiha (2000), _Heuristics for cardinality constrained portfolio optimisation_ in Computers & Operations Research, Volume 27, Pages 1271-1302. DOI: [10.1016/S0305-0548(99)00074-X].

6. Nilgun A. Canakgoz and John E. Beasley (July 2009), _Mixed-integer programming approaches for index tracking and enhanced indexation_ in European Journal of Operational Research, Volume 196, Pages 384-399. DOI: [10.1016/j.ejor.2008.03.015]

7. Eugene F. Fama and Kenneth R. French (December 2023), _Production of U.S. Rm-Rf, SMB, and HML in the Fama-French Data Library_ in Chicago Booth Research Paper, No. 23-22, Fama-Miller Working Paper. DOI: [10.2139/ssrn.4629613]. Through [Fama and French] a more up to date version of this dataset could be obtained, though lacking the adjustments made by Bruni _et al._ [2].

<!-- License Links -->
[CC-BY 4.0]: https://creativecommons.org/licenses/by/4.0/
[GPL v3+]: https://www.gnu.org/licenses/gpl-3.0.en.html
[MIT]: https://choosealicense.com/licenses/mit
[Unknown]: #licenses

<!-- DOI Links -->
[10.1016/j.dib.2016.06.031]: https://dx.doi.org/10.1016/j.dib.2016.06.031
[10.32614/CRAN.package.FRAPO]: https://dx.doi.org/10.32614/CRAN.package.FRAPO
[10.1007/s10287-014-0210-1]: https://dx.doi.org/10.1007/s10287-014-0210-1
[10.1016/j.ejor.2008.03.015]: https://dx.doi.org/10.1016/j.ejor.2008.03.015
[10.2139/ssrn.4629613]: https://dx.doi.org/10.2139/ssrn.4629613
[10.1016/S0305-0548(99)00074-X]: https://doi.org/10.1016/S0305-0548(99)00074-X
[another]: https://doi.org/10.1016/j.eswa.2011.04.233

<!-- Stock Exchange definitions -->
[DAX 100]: https://en.wikipedia.org/wiki/HDAX
[DJIA]: https://en.wikipedia.org/wiki/Dow_Jones_Industrial_Average
[EUR]: https://en.wikipedia.org/wiki/Euro
[EURO STOXX 50]: https://en.wikipedia.org/wiki/EURO_STOXX_50
[Fama and French]: https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html
[FTSE 100]: https://en.wikipedia.org/wiki/FTSE_100_Index
[gold]: https://en.wikipedia.org/wiki/Gold#Monetary_use
[HSI]: https://en.wikipedia.org/wiki/Hang_Seng_Index
[Borsa Italiana]: https://en.wikipedia.org/wiki/MIBTel
[NASDAQ 100]: https://en.wikipedia.org/wiki/Nasdaq-100
[NASDAQ Composite]: https://en.wikipedia.org/wiki/Nasdaq_Composite
[Nikkei 225]: https://en.wikipedia.org/wiki/Nikkei_225
[S&P 100]: https://en.wikipedia.org/wiki/S%26P_100
[S&P 500]: https://en.wikipedia.org/wiki/S%26P_500

<!-- Internal dataset links -->
[`DowJones`]: Datasets/DowJones
[`ESCBFX`]: Datasets/ESCBFX
[`EuroStoxx50`]: Datasets/EuroStoxx50
[`FF49Industries`]: Datasets/FF49Industries
[`FTSE100`]: Datasets/FTSE100
[`INDTRACK1`]: Datasets/INDTRACK1
[`INDTRACK2`]: Datasets/INDTRACK2
[`INDTRACK3`]: Datasets/INDTRACK3
[`INDTRACK4`]: Datasets/INDTRACK4
[`INDTRACK5`]: Datasets/INDTRACK5
[`INDTRACK6`]: Datasets/INDTRACK6
[`MIBTEL`]: Datasets/MIBTEL
[`Markowitz`]: Datasets/Markowitz
[`MultiAsset`]: Datasets/MultiAsset
[`NASDAQ100`]: Datasets/NASDAQ100
[`NASDAQComp`]: Datasets/NASDAQComp
[`SP500`]: Datasets/SP500
[`StockIndex`]: Datasets/StockIndex
[`StockIndexAdj`]: Datasets/StockIndexAdj
[`StockIndexAdjD`]: Datasets/StockIndexAdjD
