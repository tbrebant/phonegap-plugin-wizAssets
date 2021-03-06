# phonegap-plugin-wizAssets 

- PhoneGap Version : 3.0
- last update : 23/05/2016

# Description

PhoneGap plugin for managing application assets with javascript asset maps. Includes downloadFile, getFileURI, getFileURIs, deleteFile, deleteFiles.


## Install (with Plugman) 

	cordova plugin add https://github.com/Wizcorp/phonegap-plugin-wizAssets
	cordova build ios
	
	< or >
	
	phonegap local plugin add https://github.com/Wizcorp/phonegap-plugin-wizAssets
	phonegap build ios



## APIs

### wizAssets.downloadFile(remoteURL, assetId, success, fail)

- downloads a file to native App directory @ ./ + gameDir+ / + assetId <br />
- A success returns a local URL string like; file://documents/settings/img/cards/card001.jpg <br />
- An error returns an error object such as:
```
{
    "code": WizAssetsError value,
    "status": if code is a HTTP_REQUEST_ERROR, the status of the HTPP request, optional
    "message": description of the error if any
}
```

**Example**
``` 
wizAssets.downloadFile("http://google.com/logo.jpg", "img/ui/logo.jpg", successCallback, failCallback);
```

### wizAssets.deleteFile(assetId, success, fail)

- deletes the file specified by the asset id <br />
- if the asset id does not exist fail will be called with error NotFoundError <br />
- if the asset id cannot be deleted (i.e. file resides in read-only memory) fail will be called with an error message


**Example**
```
wizAssets.deleteFile("img/cards/card001.jpg", successCallback, failCallback);
```

### wizAssets.deleteFiles(assetIds, success, fail)

- delete files specified by their asset id in Array <br />
- if an asset id uses a path format and matches a folder, then the folder content will be deleted; img/cards <br />
- if an asset id cannot be deleted (i.e. file resides in read-only memory) fail will be called with an error message
- the array CAN contain one asset id

**Example**
```
wizAssets.deleteFiles(["img/cards/card001.jpg", "img/cards/card002.jpg"], successCallback, failCallback);
```

### wizAssets.getFileURI(assetId, success, fail)

- A success returns a local URL string like file://documents/settings/img/cards/card001.jpg <br />
- A failure returns an error message

**Example**
```
wizAssets.getFileURI("img/ui/logo.jpg", successCallback, failCallback);
```

### wizAssets.getFileURIs(success, fail)

- A success returns a hashmap of asset id matching its local URL such as
- A failure returns an error message

```
{

    "img/ui/loader.gif"  : "/sdcard/<appname>/img/ui/loading.gif", 
    "img/cards/card001.jpg" : "file://documents/settings/img/cards/card001.jpg" 

}
```

**Example**
```
wizAssets.getFileURIs(successCallback, failCallback);
```

### Error handling

All error codes are available via ```window.WizAssetsError```.

**Example**
```
function fail(error) {
    if (error.code === window.WizAssetsError.HTTP_REQUEST_ERROR) {
        // Check error.status
    } else {
        // Log error code with error.message
    }
}
```

| Code | Constant                     | Description                                                                                            |
|-----:|:-----------------------------|:-------------------------------------------------------------------------------------------------------|
|    1 | `ARGS_MISSING_ERROR`         | Arguments are missing in your call to WizAssets' method                                                |
|    2 | `INVALID_URL_ERROR`          | Specified URL to download is invald                                                                    |
|    3 | `CONNECTIVITY_ERROR`         | Download could not be processed because network could not be accessed                                  |
|    4 | `HTTP_REQUEST_ERROR`         | HTTP status of the request is not successful (not 2XX)                                                 |
|    5 | `HTTP_REQUEST_CONTENT_ERROR` | Content received in HTTP request could not be read                                                     |
|    6 | `DIRECTORY_CREATION_ERROR`   | Creation of the directory to save the asset failed                                                     |
|    7 | `FILE_CREATION_ERROR`        | Saving the asset failed                                                                                |
|    8 | `JSON_CREATION_ERROR`        | Your call to WizAssets' method failed and its error could not be processed internally                  |