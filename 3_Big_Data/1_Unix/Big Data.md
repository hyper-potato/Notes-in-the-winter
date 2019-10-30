# Big data Intro

Conceptual: Concept of Big Data
>  Big Data: “large volumes of high velocity, complex, and variable data that require advanced techniques and technologies to enable the capture, storage, distribution, management and analysis of the information


drivers of big data, 

structure, unstructured, vs semi structured data, , the data transfer problem, and how/why big data can help.


## Five Vs
* Volume 

* Velocity 

* Variety
  - Multiple data types, including structured, semi-structured and unstructured data.
  - Different forms: 
    - Structured: transactional
    - Semi-structured: sensor data, logs, RFID 
    - Unstructured: reviews, images, tweets, audio, video
  - Inside or outside of enterprises
    - Internal: transactional systems, server logs, emails, chats, etc
    - External: social media, sensor networks, weather data, geographic data, census, macroeconomic data, third party data providers

* Veracity 
refers to data uncertainty
– Large volumes of disparate data being ingested at high speed are only useful if the information is correct. Incorrectly indexed data or spelling mistakes could make complete datasets useless and thus the veracity is important.

* Value
Big data has many valuable applications
Value is a multifaceted property of big data. As the volume of data grows the incremental value of each data point begins to decrease. As the variety of data available increases, not all the data may aid in product development, sales, or system management. Big data is not the retention of all data; some data needs to remain volatile.


## Data size units
*   50% consider datasets between Terabyte and Petabye to be big.
*   Whatever is considered “high volume” today will be even higher tomorrow.
KB ➡️ MB  ➡️ TB  ➡️ PB  ➡️ EB  ➡️  ZB  ➡️  YB
![image.png](https://i.loli.net/2019/10/21/Rl2OphqCgQm7kSi.png)



## Linux and Cloudera
When you first log on to our Cloudera VM (as a user `cloudera`), the current directory is `/home/cloudera`


(By default your current directory is your home directory, located at `/home/cloudera`.)

The path with reference to root directory is called **absolute**. The path with reference to current directory is called **relative**.

Suppose your current directory is your home directory, and you want to list the content of a subdirectory "data" in the vagrant directory (which is is under the root). Which of the following is the right way?


   `ls /vagrant/data`



You Answered

   ls ~/vagrant/data

because the folder is not in our current directory, your best choice is to use the absolute path that starts with "/"
