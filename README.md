#highcharts-spring-ajax
###This is a sample application that shows how to send server-side data to </b>Highcharts</b> (http://www.highcharts.com/) in the
JSON format using a Spring controller.

The project is based on maven so upon downloading the project you can do a
"mvn clean package tomcat7:run" to build and deploy the app to tomcat 7. To access the application open a browser and 
go to "http://localhost:8080/highcharts" There should be 3 charts being displayed. 

The project uses:
======

-Spring 3.2.4.RELEASE (http://www.springsource.org/)
-Jackson 1.9.7 (http://jackson.codehaus.org/)
-Bootswatch 3.0 (http://bootswatch.com/)
-Highcharts 3.0.5 (http://www.highcharts.com/download)
-JQuery 1.10.2 (http://jquery.com/)


General Overview:
======
Upon accessing "http://localhost:8080/highcharts" from the browser a Spring controller `HighChartsController` is called
and a JSP file called `charts.jsp` is loaded on the browser. Once the loading is complete 3 ajax calls are made back to
the `HighChartsController` to get the data to display the charts. The `HighChartsController` in turns call a fake
service `ChartService` that generates the fake data for the chats to be generated by `Highcharts`. The data is stored in
a java POJO `DataBean` and it is sent back to the controller `HighChartsController`. The `DataBean` is then magically
converted into a JSON string by Spring. Actually Spring uses Jackson to serialize the `DataBean` to a string. The string
is sent back to custom javascript file `custom-chart.js` that has all the plumbing to feed the data to `Highcharts`.
`Highcharts` then renders the charts.

custom-chart.js Overview:
======

When the charts.jsp file has rendered on the screen it immediately kicks off some java scripts that makes 3 remote
Ajax calls to get 3 JSON string of data to be used by `Highcharts` to generate the charts. On the `charts.jsp` the
snippet of javascript that start the ball rolling is shown below. The `createNewLineChart` function create a
`Highcharts` options that get populated from the JSON that is returned from the server side. `getRemoteDataDrawChart`
function gets the remote data, populate the options object and draws the chart to a <div> with id `chart1-container`
or `chart2-container` or `chart3-container`. All the javascript code can be found in the `custom-chart.js` file.
There is a `getBaseChart` function that has a base options object that get extended by the `createNewLineChart`
function for each chart. This is especially useful if you need to display different types of charts on a single page.
Hope this will be helpful as I had a hard time finding a Spring application that integrated `Highcharts`.

  
<pre>
    var contextPath = '<c:out value="${pageContext.request.contextPath}"/>';
    $(document).ready(function() {
        getRemoteDataDrawChart(contextPath + '/linechart1', createNewLineChart('chart1-container'));
        getRemoteDataDrawChart(contextPath + '/linechart2', createNewLineChart('chart2-container'));
        getRemoteDataDrawChart(contextPath + '/linechart3', createNewLineChart('chart3-container'));
    });
</pre>
    
    
  

