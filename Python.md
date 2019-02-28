Numpy arrays can have an atrbitrary number of dimensions. Indexing and slicing works as it works in Python lists, but requires commas for multi-dimensional access. Most np functions apply to np arrays.

Numpy Arrays, Python Lists, and Pandas Series all offer index slicing collection[start(inclusive):stop(exclusive):step(negative to reverse)]

Do linear algebra with numpy arrays and matrices. They support dot products, cross products, matrix algebra, and so forth. Use this to calculate mean and variance in returns.

Number of columns of the matrix on the left must be equal to the number of rows of the matrix on the right.

(m x n) \* (n x p) = (m x p) as n = n

NaN, or not a number, means that a value is wrongly formatted or doesn’t exist. `​`

`ix = ~np.isnan(vector) #logical not gets indices not NaN`

`​NaN_free_vector = v[ix]`

Pandas:

* Pandas provides the dataframe object to store multidimensional data.
* A Pandas Series is a 1D structure that can be build from list or nparray, and is most often used for time series data
* Pandas can create an index of datetimes over an arbitrary set of datetimes and frequencies, as long as the series can be evenly divided by the frequency
* Use iloc to reference a series by index
* Use loc when you’ve indexed by something other than integers, like dates, or to filter by a boolean
  * `​s.loc[s < 3]`
* fillna() lets you fill na values, but it is usually better to dropna() to get rid of them
* pandas gives rolling means and standard deviations, pd.rolling\_mean(series, days)
* accessing dataframe columns returns series
* use colons in loc to access, like .loc(:, col\_name) or .loc(row\_name, :)
* use .loc to append and edit as well, by writing data as if a dictionary