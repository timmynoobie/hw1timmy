## SparkML

### Installation

http://spark.apache.org/downloads.html

### Before working with API

1. Download data you want to train into your spark data directory.

2. Know and understand Machine Learning method you want to apply



### General steps working with API

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
      
 
 # Now you are done with importing library, let's start second step 
 
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
       
 # Good job on successfully importing and parsing data, let's move on to building the model
  4.
 

 

    
