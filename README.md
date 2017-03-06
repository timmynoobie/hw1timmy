## SparkML

### Installation

http://spark.apache.org/downloads.html

### In this tutorial we are working with KMeans clustering Model, Logistic regression Model and Decision tree classfication/regression Model.  It will be helpful if you go can go throught the following slide to understand how each model works.



### Before working with API

1. Download data you want to train into your spark data directory.

2. Know and understand Machine Learning method you want to apply to your data

3. Open spark mllib documentation:

        https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.Accumulable
        
   Here you will find all the information you will need     


### General steps working with Spark Mllib API

1. Import correct library:

    a) you will almost always need to import Vector library to help you with data structure conversion:
    
        import org.apache.spark.mllib.linalg.Vectors
        
      Sometimes you might need to import library to help you with special structure that contains label:
      
        import org.apache.spark.mllib.regression.LabeledPoint
            
    b) Then you will need to import model library:
    
      KMeans example:
        
        import org.apache.spark.mllib.clustering.{KMeans, KMeansModel}
      
      Logistic Regression example:
        
        import org.apache.spark.mllib.classification.{LogisticRegressionModel, LogisticRegressionWithLBFGS}
      
 
 #### Now you are done with importing library, let's start second step 
 
2.  Import data from your spark directory:
 
    KMeans example:
 
            val data = sc.textFile("data/mllib/kmeans_data.txt")
            
    Here we declare our input file is imported into "data". And it is a "textFile". Feel free to change the path
    to sc.textFile("yourdata_path.txt").
3. Parse the data to JavaRDD structure because most of models require it:
 
    KMeans example:
       
            val parsedData = data.map(s=>Vecctors.dense(s.split(' ').map(_.toDouble))).cache()
            
     Now we are using "Vectors" to split data with space ' ' into double. Each line will be seen as a vector.
     Remember to inspect your .txt file before-hand because your element might be split by ',' or other characters.
       
 #### Good job on successfully importing and parsing data, let's move on to building the model
  
  4. Declare a model with the parameters you want: (You may want to check your options on spark mllib documentation)
  
     KMeans example:
     
            val clusters = KMeans.train(parsedData, 3, 100)
            
     We are using KMeans moel and declare it as clusters. It has 3 cluster-centers and runs 100 iterations.
  
  5. If you want save the model and use for later:
  
     KMeans example:
     
            cluster.save(sc, "your path here")
            
            val newmodel = KMeansModel.load(sc, "your path here")
            
      Here, "cluster" is our model name and we load as a KMeans model "newmodel".
      
 #### You have completed the basic steps of creating model. Next, we will help you to explore some useful functions of KMeans and Logistic Regression model. You may find more in spark mllib documentation
 
   1. KMean example:
            
        Find cluster center/mean:
            
             clusters.clusterCenters(1)
             
        Find square error of KMean model:
        
             val WSSSE = clusters.computerCost(parsedData)
             
        Predict a data set. This returns which center each element grouping to:
        
             val test = clusters.predict(parsedData)
             
        Print out results:
        
             test.collect()
             
             
    
   2. Logistic regression example:
   
   
        Help to split data to train and test set. This is mostly used for multiple-fold cross validation:
        
            val splits = data.randomSplit(Array(0.6, 0.4), seed = 11L)
            val training = splits(0).cache()
            val test = splits(1)
        Here we are spliting data to 60% and 40% parts. And we are using 60% part for training and 40% parts for testing 
   
   
        Build your Prediction vs Actual data model:
        
            val predictionAndLabels = test.map { case LabeledPoint(label, features) => val prediction = model.predict(features) (prediction, label)}
        We put our prediction and actual data set into pair (prediction, actual).
        
        
        Inspect Prediction vs Actual data:
        
             predictionAndLabels.collect()
             
        Building a accuracy testing method might help with estimating accuracy:
            
            import org.apache.spark.mllib.evaluation.MulticlassMetrics
            val metrics = new MulticlassMetrics(predictionAndLabels)
            val accuracy = metrics.accuracy
            
        Here we use a evaluation library to help. And the "MulticlassMetrics" helps to calculate the accuracy of our model.    
   
   
   
   3. Decision tree example:
   
        
   
 

 

    
