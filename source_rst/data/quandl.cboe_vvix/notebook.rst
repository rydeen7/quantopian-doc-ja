CBOE VVIX Index
===============

In this notebook, we’ll take a look at the CBOE VVIX Index dataset,
available on the `Quantopian
Store <https://www.quantopian.com/store>`__. This dataset spans 03 Jan
2007 through the current day, and documents the expected volatility of
the 30-day forward price of the VIX. Calculated by CBOE, provided by
Quandl.

Notebook Contents
-----------------

There are two ways to access the data and you’ll find both of them
listed below. Just click on the section you’d like to read through.

-  Interactive overview: This is only available on Research and uses
   blaze to give you access to large amounts of data. Recommended for
   exploration and plotting.
-  Pipeline overview: Data is made available through pipeline which is
   available on both the Research & Backtesting environment. Recommended
   for custom factor development and moving back & forth between
   research/backtesting.

Limits
~~~~~~

One key caveat: we limit the number of results returned from any given
expression to 10,000 to protect against runaway memory usage. To be
clear, you have access to all the data server side. We are limiting the
size of the responses back from Blaze.

With preamble in place, let’s get started:

#Interactive Overview ### Accessing the data with Blaze and Interactive
on Research Partner datasets are available on Quantopian Research
through an API service known as `Blaze <http://blaze.pydata.org>`__.
Blaze provides the Quantopian user with a convenient interface to access
very large datasets, in an interactive, generic manner.

Blaze provides an important function for accessing these datasets. Some
of these sets are many millions of records. Bringing that data directly
into Quantopian Research directly just is not viable. So Blaze allows us
to provide a simple querying interface and shift the burden over to the
server side.

It is common to use Blaze to reduce your dataset in size, convert it
over to Pandas and then to use Pandas for further computation,
manipulation and visualization.

Helpful links: \* `Query building for
Blaze <http://blaze.readthedocs.io/en/latest/queries.html>`__ \*
`Pandas-to-Blaze
dictionary <http://blaze.readthedocs.io/en/latest/rosetta-pandas.html>`__
\* `SQL-to-Blaze
dictionary <http://blaze.readthedocs.io/en/latest/rosetta-sql.html>`__.

| Once you’ve limited the size of your Blaze object, you can convert it
  to a Pandas DataFrames using: > ``from odo import odo``
| > ``odo(expr, pandas.DataFrame)``

###To see how this data can be used in your algorithm, search for the
``Pipeline Overview`` section of this notebook or head straight to
Pipeline Overview

.. code:: ipython2

    # For use in Quantopian Research, exploring interactively
    from quantopian.interactive.data.quandl import cboe_vvix as dataset
    
    # import data operations
    from odo import odo
    # import other libraries we will use
    import pandas as pd

.. code:: ipython2

    # Let's use blaze to understand the data a bit using Blaze dshape()
    dataset.dshape




.. parsed-literal::

    dshape("var * {vvix: float64, asof_date: datetime, timestamp: datetime}")



.. code:: ipython2

    # And how many rows are there?
    # N.B. we're using a Blaze function to do this, not len()
    dataset.count()




.. raw:: html

    2364



.. code:: ipython2

    # Let's see what the data looks like. We'll grab the first three rows.
    dataset[:3]




.. raw:: html

    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>vvix</th>
          <th>asof_date</th>
          <th>timestamp</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>89.67</td>
          <td>2016-02-23</td>
          <td>2016-02-24 12:00:26.399143</td>
        </tr>
        <tr>
          <th>1</th>
          <td>86.74</td>
          <td>2016-02-24</td>
          <td>2016-02-25 12:00:47.967020</td>
        </tr>
        <tr>
          <th>2</th>
          <td>83.51</td>
          <td>2016-02-25</td>
          <td>2016-02-26 12:01:40.620973</td>
        </tr>
      </tbody>
    </table>



Let’s go over the columns: - **vvix**: volatility of VIX -
**asof_date**: the timeframe to which this data applies - **timestamp**:
this is our timestamp on when we registered the data.

We’ve done much of the data processing for you. Fields like
``timestamp`` are standardized across all our Store Datasets, so the
datasets are easy to combine.

We can select columns and rows with ease. Below, we’ll do a simple plot
of VVIX since 2007.

.. code:: ipython2

    # Plotting this DataFrame since 2007
    df = odo(dataset, pd.DataFrame)
    df.head(5)




.. raw:: html

    <div style="max-height:1000px;max-width:1500px;overflow:auto;">
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>vvix</th>
          <th>asof_date</th>
          <th>timestamp</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>89.67</td>
          <td>2016-02-23</td>
          <td>2016-02-24 12:00:26.399143</td>
        </tr>
        <tr>
          <th>1</th>
          <td>86.74</td>
          <td>2016-02-24</td>
          <td>2016-02-25 12:00:47.967020</td>
        </tr>
        <tr>
          <th>2</th>
          <td>83.51</td>
          <td>2016-02-25</td>
          <td>2016-02-26 12:01:40.620973</td>
        </tr>
        <tr>
          <th>3</th>
          <td>83.72</td>
          <td>2016-02-26</td>
          <td>2016-02-29 12:00:32.099177</td>
        </tr>
        <tr>
          <th>4</th>
          <td>85.06</td>
          <td>2016-02-29</td>
          <td>2016-03-01 12:01:19.224513</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython2

    # So we can plot it, we'll set the index as the `asof_date`
    df['asof_date'] = pd.to_datetime(df['asof_date'])
    df = df.set_index(['asof_date'])
    df.head(5)




.. raw:: html

    <div style="max-height:1000px;max-width:1500px;overflow:auto;">
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>vvix</th>
          <th>timestamp</th>
        </tr>
        <tr>
          <th>asof_date</th>
          <th></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>2016-02-23</th>
          <td>89.67</td>
          <td>2016-02-24 12:00:26.399143</td>
        </tr>
        <tr>
          <th>2016-02-24</th>
          <td>86.74</td>
          <td>2016-02-25 12:00:47.967020</td>
        </tr>
        <tr>
          <th>2016-02-25</th>
          <td>83.51</td>
          <td>2016-02-26 12:01:40.620973</td>
        </tr>
        <tr>
          <th>2016-02-26</th>
          <td>83.72</td>
          <td>2016-02-29 12:00:32.099177</td>
        </tr>
        <tr>
          <th>2016-02-29</th>
          <td>85.06</td>
          <td>2016-03-01 12:01:19.224513</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython2

    # Plotting the VVIX
    import matplotlib.pyplot as plt
    df.vvix.plot(label=str(dataset))
    plt.ylabel(str(dataset))
    plt.legend()
    plt.title("Graphing %s since %s" % (str(dataset), min(df.index)))




.. parsed-literal::

    <matplotlib.text.Text at 0x7f03a415da90>




.. image:: notebook_files/notebook_8_1.png


#Pipeline Overview

Accessing the data in your algorithms & research
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The only method for accessing partner data within algorithms running on
Quantopian is via the pipeline API. Different data sets work differently
but in the case of this data, you can add this data to your pipeline as
follows:

Import the data set here >
``from quantopian.pipeline.data.quandl import cboe_vvix``

Then in intialize() you could do something simple like adding the raw
value of one of the fields to your pipeline: >
``pipe.add(cboe_vvix.vvix.latest, 'vvix')``

Pipeline usage is very similar between the backtester and Research so
let’s go over how to import this data through pipeline and view its
outputs.

.. code:: ipython2

    # Import necessary Pipeline modules
    from quantopian.pipeline import Pipeline
    from quantopian.research import run_pipeline
    from quantopian.pipeline.factors import AverageDollarVolume

.. code:: ipython2

    # Import the datasets available
    from quantopian.pipeline.data.quandl import cboe_vvix

Now that we’ve imported the data, let’s take a look at which fields are
available for each dataset.

You’ll find the dataset, the available fields, and the datatypes for
each of those fields.

.. code:: ipython2

    print "Here are the list of available fields per dataset:"
    print "---------------------------------------------------\n"
    
    def _print_fields(dataset):
        print "Dataset: %s\n" % dataset.__name__
        print "Fields:"
        for field in list(dataset.columns):
            print "%s - %s" % (field.name, field.dtype)
        print "\n"
    
    _print_fields(cboe_vvix)
    
    
    print "---------------------------------------------------\n"


.. parsed-literal::

    Here are the list of available fields per dataset:
    ---------------------------------------------------
    
    Dataset: cboe_vvix
    
    Fields:
    vvix - float64
    
    
    ---------------------------------------------------
    


Now that we know what fields we have access to, let’s see what this data
looks like when we run it through Pipeline.

This is constructed the same way as you would in the backtester. For
more information on using Pipeline in Research view this thread:
https://www.quantopian.com/posts/pipeline-in-research-build-test-and-visualize-your-factors-and-filters

.. code:: ipython2

    pipe = Pipeline()
           
    pipe.add(cboe_vvix.vvix.latest, 'vvix')

.. code:: ipython2

    # Setting some basic liquidity strings (just for good habit)
    dollar_volume = AverageDollarVolume(window_length=20)
    top_1000_most_liquid = dollar_volume.rank(ascending=False) < 1000
    
    pipe.set_screen(top_1000_most_liquid & cboe_vvix.vvix.latest.notnan())

.. code:: ipython2

    # The show_graph() method of pipeline objects produces a graph to show how it is being calculated.
    pipe.show_graph(format='png')




.. image:: notebook_files/notebook_17_0.png



.. code:: ipython2

    # run_pipeline will show the output of your pipeline
    pipe_output = run_pipeline(pipe, start_date='2013-11-01', end_date='2013-11-25')
    pipe_output




.. raw:: html

    <div style="max-height:1000px;max-width:1500px;overflow:auto;">
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th></th>
          <th>vvix</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th rowspan="30" valign="top">2013-11-01 00:00:00+00:00</th>
          <th>Equity(21 [AAME])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(25 [AA_PR])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(117 [AEY])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(225 [AHPI])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(312 [ALOT])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(392 [AMS])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(468 [API])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(548 [ASBI])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(717 [BAMM])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(790 [BDL])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(880 [BIO_B])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(925 [BKSC])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(1088 [BRID])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(1095 [BRN])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(1157 [BTUI])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(1190 [BWIN_A])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(1193 [BWL_A])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(1323 [CAW])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(1653 [MOC])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(1668 [CMS_PRB])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(1988 [CUO])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(2078 [DAIO])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(2103 [ESCR])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(2124 [DD_PRA])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(2209 [DGSE])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(2292 [DRCO])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(2344 [DRAM])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(2382 [DXR])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(2389 [COBR])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>Equity(2391 [DYNT])</th>
          <td>69.56</td>
        </tr>
        <tr>
          <th>...</th>
          <th>...</th>
          <td>...</td>
        </tr>
        <tr>
          <th rowspan="30" valign="top">2013-11-25 00:00:00+00:00</th>
          <th>Equity(45179 [ERW])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45195 [LGL_WS])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45203 [NASH])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45222 [QPAC_U])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45240 [INTL_L])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45270 [TIPT])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45288 [EMHD])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45301 [TRC_WS])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45390 [CPXX])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45412 [EAGL])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45414 [EAGL_W])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45420 [ROIQ_U])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45432 [SPCB])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45510 [MLPC])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45524 [NVEE])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45525 [NVEE_W])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45527 [JASN])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45536 [JASN_W])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45562 [ESBA])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45563 [OGCP])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45564 [FISK])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45646 [CHNA])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45678 [SLQD])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45680 [ADXS_W])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45717 [FTGC])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45768 [KODK_WS])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45792 [FTSD])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45824 [ROIQ_W])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45854 [PGAL])</th>
          <td>63.77</td>
        </tr>
        <tr>
          <th>Equity(45895 [EMSH])</th>
          <td>63.77</td>
        </tr>
      </tbody>
    </table>
    <p>16983 rows × 1 columns</p>
    </div>



Here, you’ll notice that each security is mapped to VVIX. So you could
grab any security to obtain the value of VVIX.

Taking what we’ve seen from above, let’s see how we’d move that into the
backtester.

.. code:: ipython2

    # This section is only importable in the backtester
    from quantopian.algorithm import attach_pipeline, pipeline_output
    
    # General pipeline imports
    from quantopian.pipeline import Pipeline
    from quantopian.pipeline.factors import AverageDollarVolume
    
    # For use in your algorithms via the pipeline API
    from quantopian.pipeline.data.quandl import cboe_vvix
    
    def make_pipeline():
        # Create our pipeline
        pipe = Pipeline()
        
        # Screen out penny stocks and low liquidity securities.
        dollar_volume = AverageDollarVolume(window_length=20)
        is_liquid = dollar_volume.rank(ascending=False) < 1000
        
        # Create the mask that we will use for our percentile methods.
        base_universe = (is_liquid)
    
        # Add the datasets available
        pipe.add(cboe_vvix.vvix.latest, 'vvix')
    
        # Set our pipeline screens
        pipe.set_screen(is_liquid)
        return pipe
    
    def initialize(context):
        attach_pipeline(make_pipeline(), "pipeline")
        
    def before_trading_start(context, data):
        results = pipeline_output('pipeline')

Now you can take that and begin to use it as a building block for your
algorithms, for examples on how to do that you can clone the sample
algorithm or visit our data pipeline factor library
