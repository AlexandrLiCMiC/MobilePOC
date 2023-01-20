function TestRail_OnStopTestCase(Sender, StopTestCaseParams)
{
  
 var RunID = ProjectSuite.Variables.RunID;
 var TestCaseID = ProjectSuite.Variables.TestCaseID;
 var RecordIdx;
 var address = "https://cmic.testrail.io/index.php?/api/v2/add_result_for_case/" + RunID  + "/" +  TestCaseID;
 var username = "";
 var password = "";
  
    
  ProjectSuite.Variables.TestRail.Reset();
  for(RecordIdx = 1; RecordIdx <= 1; RecordIdx++)
  {
    //username from CSV file
    username = ProjectSuite.Variables.TestRail.Value("Username");
    //password from CSV file
    password = ProjectSuite.Variables.TestRail.Value("Password");
  }  
 

   // Convert the user credentials to base64 for preemptive authentication
  var credentials = aqConvert.VarToStr(dotNET.System.Convert.ToBase64String
    (dotNET.System_Text.Encoding.UTF8.GetBytes_2(username + ":" + password)));
 

  var request = aqHttp.CreatePostRequest(address);
  request.SetHeader("Authorization", "Basic " + credentials);
  request.SetHeader("Content-Type", "application/json");
 

  var statusId;
  var comment = "";
  switch (StopTestCaseParams.Status)
  {
    case 0: // lsOk
      statusId = 1;
      comment = "Passed through TestComplete"; // Passed
      break;
    case 1: // lsWarning
      statusId = 4; // Passed with a warning
      comment = StopTestCaseParams.FirstWarningMessage;
      break;
    case 2: // lsError
      statusId = 5; // Failed
      comment = StopTestCaseParams.FirstErrorMessage;
      break;
  }

  var requestBody =
  {
    "status_id": statusId,
    "comment": comment
  };
  var response = request.Send(JSON.stringify(requestBody));
  
  
  if (response.StatusCode != aqHttpStatusCode.OK)
  {
    Log.Warning("Failed to send results to TestRail. See the Details in the previous message.");
  } 
  
  
}