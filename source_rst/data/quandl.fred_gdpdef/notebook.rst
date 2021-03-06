Quandl: US GDP, Implicit Price Deflator
=======================================

In this notebook, we’ll take a look at data set , available on
`Quantopian <https://www.quantopian.com/data>`__. This dataset spans
from 1947 through the current day. It contains the value for the United
States GDP, using the implicit price deflator by the US Federal Reserve
via the `FRED data
initiative <https://research.stlouisfed.org/fred2/>`__. We access this
data via the API provided by `Quandl <https://www.quandl.com>`__. `More
details <https://www.quandl.com/data/FRED/GDPDEF>`__ on this dataset can
be found on Quandl’s website.

Blaze
~~~~~

Before we dig into the data, we want to tell you about how you generally
access Quantopian partner data sets. These datasets are available using
the `Blaze <http://blaze.pydata.org>`__ library. Blaze provides the
Quantopian user with a convenient interface to access very large
datasets.

Some of these sets (though not this one) are many millions of records.
Bringing that data directly into Quantopian Research directly just is
not viable. So Blaze allows us to provide a simple querying interface
and shift the burden over to the server side.

To learn more about using Blaze and generally accessing Quantopian
partner data, clone `this tutorial
notebook <https://www.quantopian.com/clone_notebook?id=561827d21777f45c97000054>`__.

With preamble in place, let’s get started:

.. code:: ipython2

    # import the dataset
    from quantopian.interactive.data.quandl import fred_gdpdef
    # Since this data is public domain and provided by Quandl for free, there is no _free version of this
    # data set, as found in the premium sets. This import gets you the entirety of this data set.
    
    # import data operations
    from odo import odo
    # import other libraries we will use
    import pandas as pd
    import matplotlib.pyplot as plt

.. code:: ipython2

    fred_gdpdef.sort('asof_date')




.. raw:: html

    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>asof_date</th>
          <th>value</th>
          <th>timestamp</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>1947-01-01</td>
          <td>12.566</td>
          <td>1947-01-01</td>
        </tr>
        <tr>
          <th>1</th>
          <td>1947-04-01</td>
          <td>12.745</td>
          <td>1947-04-01</td>
        </tr>
        <tr>
          <th>2</th>
          <td>1947-07-01</td>
          <td>12.957</td>
          <td>1947-07-01</td>
        </tr>
        <tr>
          <th>3</th>
          <td>1947-10-01</td>
          <td>13.276</td>
          <td>1947-10-01</td>
        </tr>
        <tr>
          <th>4</th>
          <td>1948-01-01</td>
          <td>13.379</td>
          <td>1948-01-01</td>
        </tr>
        <tr>
          <th>5</th>
          <td>1948-04-01</td>
          <td>13.497</td>
          <td>1948-04-01</td>
        </tr>
        <tr>
          <th>6</th>
          <td>1948-07-01</td>
          <td>13.747</td>
          <td>1948-07-01</td>
        </tr>
        <tr>
          <th>7</th>
          <td>1948-10-01</td>
          <td>13.789</td>
          <td>1948-10-01</td>
        </tr>
        <tr>
          <th>8</th>
          <td>1949-01-01</td>
          <td>13.717</td>
          <td>1949-01-01</td>
        </tr>
        <tr>
          <th>9</th>
          <td>1949-04-01</td>
          <td>13.579</td>
          <td>1949-04-01</td>
        </tr>
        <tr>
          <th>10</th>
          <td>1949-07-01</td>
          <td>13.509</td>
          <td>1949-07-01</td>
        </tr>
      </tbody>
    </table>



The data goes all the way back to 1947 and is updated quarterly.

Blaze provides us with the first 10 rows of the data for display. Just
to confirm, let’s just count the number of rows in the Blaze expression:

.. code:: ipython2

    fred_gdpdef.count()




.. raw:: html

    814



Let’s go plot it for fun. This data set is definitely small enough to
just put right into a Pandas DataFrame

.. code:: ipython2

    gdpdef_df = odo(fred_gdpdef, pd.DataFrame)
    
    gdpdef_df.plot(x='asof_date', y='value')
    plt.xlabel("As Of Date (asof_date)")
    plt.ylabel("GDP Rate")
    plt.title("US GDP, Implicit Price Deflator")
    plt.legend().set_visible(False)



.. image:: notebook_files/notebook_6_0.png


