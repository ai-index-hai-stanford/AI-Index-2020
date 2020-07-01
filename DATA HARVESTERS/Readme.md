

### Data Harvesters

- Microsoft Academic Graph (MAG) harvester
- Stackoverflow Harvester

##### MAG Harvester

This is a harvester for getting research data from MAG, i.e., https://msr-apis.portal.azure-api.net/products/project-academic-knowledge. It consists of two scripts:
MAG_Conference_harvester -  https://colab.research.google.com/drive/1NfV8ahz39ylm5DkM3ZQQw07JullsAf9a?usp=sharing 

- Class MAG():<br>
        Arguments:<br>
        -----------<br>
        - conference: Name of the conference we need to fetch data.<br>
        - data_fetch: What kind of Data to fetch from MAG (We have three    options right now Publication data, Field of Study data and Author data )<br>
        - start_date: start date of data<br>
        - end_date: end date of data<br>
        - retry: Number of attempts to connect to MAG<br>
        Returns:<br>
        ---------<br>
        - self.data: A cleaned data frame containing MAG data.<br>
- Member functions<br>
1. __init__(self,**kwargs)<br>
Here we define the data members of the class:<br>
    * self.conference: the conference name provided by the user.<br> 
    * self.start_date: the start date provided by the user.<br>
    * self.end_date: The end date provided by the user.<br>
    * self.retry: Number of attempts to connect to MAG.<br>
    * self.s_d: stores the year of the start date provided.<br>
    * self.e_d: stores the year of the end date provided.<br>
    * self.data_fetch: The type of data we need to fetch is defined here according to user input.<br>
    * self.conference_id: Mapping conference name to its id and then storing it in this file. <br>
    * Mapping: <a href="https://drive.google.com/file/d/1lqLp8UUqSmdDaXQ6begbRFpct3Q3uXNh/view?usp=sharing">Conferences ID</a> <br> 

- getData(self)<br> 
  * In this function, we write queries for MAG and request some attributes based on the query and get the response from MAG.<br> 
  * First, we define three variables which are standard format in MAG.<br> 
    * Endpoint = ‘https://api.labs.cognitive.microsoft.com/’<br> 
    * Api_version = 'academic/v1.0/evaluate?'<br> 
    * Headers = {'Ocp-Apim-Subscription-Key':’Your subscription key to Project academic knowledge’}<br>
    * To generate your own key subscribe to <a href = "https://msr-apis.portal.azure-api.net/products/project-academic-knowledge">Microsoft Research API</a>.<br>
  * Here, we are using the Evaluate REST API Method from Project Academic knowledge to get the data we want. For details: <a href="https://docs.microsoft.com/en-us/academic-services/project-academic-knowledge/reference-evaluate-method">Evaluate Method</a>. <br>
  * Based on the value given for self.data_fetch we define searchstring, attributes and URL.<br>
    * Searchstring: It contains the MAG query. These are defined to get particular data. For query expression: <a href="https://docs.microsoft.com/en-us/academic-services/project-academic-knowledge/reference-query-expression-syntax">Query Expression Syntax</a>. <br>
    * Attributes: Attributes are the values we need to fetch based on the queries. Each attribute in an entity has a particular name <a href="https://docs.microsoft.com/en-us/academic-services/project-academic-knowledge/reference-entity-attributes">Entity Attributes</a>. <br>
    * URL: It is the request url.<br>
  * Publication Count<br>
    * We use Conference Instance Entity of MAG to get the publication count of each year of any of the conference listed in <a href="https://drive.google.com/file/d/1lqLp8UUqSmdDaXQ6begbRFpct3Q3uXNh/view?usp=sharing">Conferences ID</a>. <br>
    * Searchstring = And(Composite(PCS.CN==self.conference),CISD=['self.start_date','self.end_date'])
    * Attributes are CISD,PC,CIL,PCS.CN.
    * Here, PCS.CN is the conference name and CISD is the Start date of the conference instance, PC is the publication count, CIL Location of the conference instance.
    * For more information on Entities visit <a href="https://docs.microsoft.com/en-us/academic-services/project-academic-knowledge/reference-entity-attributes">Entity Attributes</a>. <br> 
  * Field of Study
    * We use Paper entities to get the Field of study data.
    * In this we fetch all the papers that are published in a particular conference by querying the conference id and the year and returning all the papers with their Affiliations, Title, Fields of study mapped to this paper.
    * Searchstring = And(Composite(PCS.CId==self.conference_id),Y=['self.s_d','self.e_d']).<br>
    * Attributes are AA.DAfN,VSN,Y,F.FN,Ti.<br>
    * Here, PCS.CId is the conference Id and  Y is the year the paper was published, AA.DAfN is the Affiliation of the paper, F.FN is the Fields of study marked to that paper, Ti is Title of that paper and VSN conference normalized name.<br>
    * For other attributes in paper entities visit: <a href="https://docs.microsoft.com/en-us/academic-services/project-academic-knowledge/reference-entity-attributes">Entity Attributes</a>. <br> 
  * Author
    * We use paper entities to get the author data. We fetch all the papers of a particular conference and query the conference id and the year and return all the papers with their Affiliations, Title, and the Author names of that paper.
    * Searchstring = And(Composite(PCS.CId==self.conference_id),Y=['self.s_d','self.e_d'])
    * Attributes are Ti,Y,AA.DAuN,AA.DAfN,VSN.
    * Here, PCS.CId is the conference Id and  Y is the year the paper was published, AA.DAfN is the Affiliation of the paper, AA.DAuN is the Authors of the paper, Ti is Title of that paper and VSN conference normalized name.
  * Once the URL is defined we generate a get request to the server,i.e, response = requests.get(url, headers=headers)
  * And store the output in response.
  * Then we store the output into a variable query: query = response.json()
  * And then we call json_to_csv() function to convert this json to csv.
  * Then we call basicPreprocess() function for preprocessing the data. 
  * Finally, we call grid function for mapping Affiliations to countries.
* json_to_csv()
  * Standard json to csv converter.
  * Input: json file
  * Output: csv file
* basicPreprocess()
  * Here we preprocess the data based on the data fetched by the user.
  * Field of study
    * Here we drop unnecessary fields and group by Date, Field_of_study, Affiliation and aggregate count to get the publication count of each affiliation in each field of study over the years.
  * Author
    * Here we just drop unnecessary fields.
* grid()
  * This function is used to map affiliations with the city, state, countries, latitude and longitude.
  * We use GRID - Global Research Identifier Database.
  * Here, first we merge address.csv and grid.csv to get the Latitude and longitude data with affiliation and country data into one dataframe and then we merge it with our dataframe.

