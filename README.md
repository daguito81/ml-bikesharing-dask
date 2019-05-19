# ml-bikesharing-dask
Reimplementation of Bike Sharing DS notebook using Dask instead  

For this assignment we will reuse the notebook we turned in for Term 2 and change the appropiate commands and functions to use Dask instead of pandas.  

For this code, the client is created inside the notebook with client = Client(). However we could connect to a cluster created manually using dask-scheduler and dask-worker if desired. We would only need to add the IP:PORT address of the scheduler inside the Client() object.  

Couple of Notes:  
1) The data was removed from github and instead it will download a copy of the csv from Azure Blob storage. This is to simulate how it would access big data from a distributed storage liek Azure BLob or AWS's S3.  
2) The first part reads the data and tries to use dask and only computing when needed. For visualization, we computed a sample of the data to simulate how we would work with big enough data that won't fit in RAM on 1 computer (As for visualization you would need all the data for a scatter plot for example.)  
3) Fot the ML part, we repartitioned the data to 1 partition. This was to simulate scattering the data between the workers and simply improve the performance by including more cores to parallelize grid searches or parallelizable algorrithms.  

For the ML part we tried 3 different things with Dask:
1) We did a native Dask Alogorithm (using dask_ml.linear_models.LinearRegression  
2) Used an sklearn and pandas dataframes but gridsearched usign dask's dask_ml.model_selection.GridSearchCV  
3) Used sklearn and pandas to create different algorithms that are not native to dask, but used joblib using dask as its backend to paralellize whichever models had the n_jobs parameter. This didn't work better in this case because we had 1 worker and joblib already parallelizes between cores. But would've given a better performance if we had more workers connected to our dask-scheduler.
