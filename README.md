# SAS Visual Analytics third party visualizations

This project contains code samples that can be used as data-driven content within a SAS Visual Analytics (VA) report. For additional information, see [Programming Considerations for Data-Driven Visualizations](http://go.documentation.sas.com/?cdcId=vacdc&cdcVersion=8.2&docsetId=varef&docsetTarget=n109mqtyl6quiun1mwfgtcn2s68b.htm&locale=en).

## util/messagingUtil.js

It contains the functions you need to send/receive messages to/from SAS Visual Analytics. You must include the following line in the _\<head\>_ of the web page:

```html
<script type="text/javascript" src="../util/messagingUtil.js"></script>
```

### setOnDataReceivedCallback

Sets a callback function to handle messages received from VA.

_Usage:_

```javascript
va.messagingUtil.setOnDataReceivedCallback(callback)
```

* `callback` is the callback function name that you must define.

### postSelectionMessage

Sends back to VA a message containing selections made in the third-party visualization. VA will use that information to either filter or select (brush) other report objects, depending on the Actions defined between the data-driven object and other VA report objects.

_Usage:_

```javascript
va.messagingUtil.postSelectionMessage(resultName, selectedRows)
```

* `resultName`is the name of the associated query result, which is obtained from the message received from VA (event.data.resultName).
* `selectedRows` is an array of numbers (e.g. `[0, 3, 4]`) or objects (e.g. `[{row: 0}, {row: 3}, {row: 4}]`) that contains the indexes of the selected rows, as they appear in event.data.data.

### postInstructionalMessage

Sends back to VA an instructional message. This message is displayed in the data-driven content object in the VA report and is useful for sending text messages back to report authors informing required roles, their assignment order, types, etc.

_Usage:_

```javascript
va.messagingUtil.postInstructionalMessage(resultName, strMessage)
```

* `resultName`is the name of the associated query result, which is obtained from the message received from VA (event.data.resultName).
* `strMessage` is the text message to be sent.

## util/contentUtil.js

It contains the functions you need to validate the data received from VA. You must include the following line in the _\<head\>_ of the web page:

```html
<script type="text/javascript" src="../util/contentUtil.js"></script>
```

### validateRoles

Checks if the data received from VA have all the columns (number, sequence, and type) required for the visualization.

_Usage:_

```javascript
isValid = va.contentUtil.validateRoles(resultData, expectedTypes, optionalTypes)
```

* `resultData` is the message received from VA (event.data).
* `expectedTypes` is an array describing the types of the columns that are required. The order is important and indicates the sequence that columns are assigned in the Roles tab in VA. Example: ["string", "number", "number"]. Valid types are "string", "number", and "date".
* `optionalTypes` is an array describing the types of the columns that are optional. The order is important and indicates the sequence that columns are assigned in the Roles tab in VA, after the required columns. Example: ["string", "number", "number"]. Because they are optional, the **number** of optional columns and optional types provided don't need to match. Type comparison will be made while there is still a column and a type to be compared, and the rest will be ignored. One or more optional columns of same type can be represented as a single type instead of an array, for example: "number" indicates that all optional columns, if existent, must be numeric columns. An empty array [] indicates their types can be anything. A value null indicates no optional columns are accepted. Valid types are "string", "number", and "date".
* Returns _true_ or _false_.

### initializeSelections

Uses the message received from VA to extract information about selections made in VA objects. After extracting selection information, the "brush" column is removed from the message.

_Usage:_

```javascript
selections = va.contentUtil.initializeSelections(resultData)
```

* `resultData` is the message received from VA (event.data).
* Returns `selections`, an array of objects containing the indexes of the selected rows (e.g. `[{row: 2}, {row: 5}]`)

## thirdPartyHelpers/google.js

It contains helper functions you most likely need with Google Charts. You must include the following lines in the _\<head\>_ of the web page:

```html
<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
<script type="text/javascript" src="../thirdPartyHelpers/google.js"></script>
```

### setupResizeListeners

Sets a callback function to handle window resizing events.

_Usage:_

```javascript
va.googleHelper.setupResizeListeners(callback)
```

* `callback` is the callback function name that you must define. That's normally the function that re-draws the chart.

### createDataTable

Uses the data and columns fields from the VA message to create a DataTable object. Google Charts take DataTable as input data for its charts, but in addition to that, DataTable offers a series of methods that can help with table manipulation. More information on DataTable can be found [here](https://developers.google.com/chart/interactive/docs/reference#DataTable).

_Usage:_

```javascript
dataTable = va.googleHelper.createDataTable(resultData)
```

* `resultData` is the message received from VA (event.data).
* Returns `dataTable`, the input data as a DataTable object.

### formatData

Uses the columns metadata within the message received from VA to update column formats in dataTable. Only numeric columns are affected. Supported VA formats are: DOLLAR, COMMA, F, and PERCENT. Other column formats are kept unchanged.

_Usage:_

```javascript
va.googleHelper.formatData(dataTable, resultData)
```

* `dataTable` is the input data as a DataTable object.
* `resultData` is the message received from VA (event.data).

## thirdPartyHelpers/d3.js

It contains helper functions you most likely need with D3 charts. You must include the following line in the _\<head\>_ of the web page:

```html
<script type="text/javascript" src="../thirdPartyHelpers/d3.js"></script>
```

### configureFormatter

Uses the columns metadata within the message received from VA to configure D3 formats. Only numeric columns are affected. Supported VA formats are: DOLLAR, COMMA, F, and PERCENT. Other columns formats are kept unchanged.

_Usage:_

```javascript
va.d3Helper.configureFormatter(resultData, formatter)
```

* `resultData` is the message received from VA (event.data).
* `formatter` (PENDING)

## thirdPartyHelpers/c3.js

It contains helper functions you most likely need with C3 charts. You must include the following line in the _\<head\>_ of the web page:

```html
<script type="text/javascript" src="../thirdPartyHelpers/c3.js"></script>
```

### configureChartData

(PENDING)

_Usage:_

```javascript
chartData = va.c3Helper.configureChartData(resultData, chartType, previousConfig)
```

* `resultData` is the message received from VA (event.data).
* `chartType` (PENDING)
* `previousConfig` (PENDING)
* Returns `chartData`, (PENDING) {rows:[[]]}