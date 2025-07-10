# Camp 100 BulletinNotificationApp

Part of the Camp 100 Digital Bulletin System.

I'm not maintaining two READMEs for this project so go over to [camp-100-bulletin-publish-app](https://github.com/ThomasBoxall/camp-100-bulletin-publish-app) to see more about what this is, what it does and why it does.

But yes, I am now going to store a useful script in this README.

## A Helper Script To Discord Message on Form Submit
Deploy as a Google Apps Script on the Google Form which is used to gather Bulletin Data. Setup with a trigger for 'on form submit'.

Obvs, populate with the correct Discord webhook and username (hidden somewhere in the middle of the code).

```js
function onFormSubmit(e) {
  // Get the form responses
  var itemResponses = e.response.getItemResponses();

  // Loop through the responses
  for (var i = 0; i < itemResponses.length; i++) {
    var itemResponse = itemResponses[i];
    var question = itemResponse.getItem().getTitle();
    var answer = itemResponse.getResponse();

    // Log the question and answer
    Logger.log(i + ': Question: ' + question + ', Answer: ' + answer);
  }
  Logger.log(`test 2: ${itemResponses[2].getResponse()}`);

  // now we build message content string
  let discordMessageString = `📨 **NEW ENTRY** from ${itemResponses[0].getResponse()} // ${itemResponses[1].getResponse()} // ${itemResponses[2].getResponse()} \nTitle: ${itemResponses[4].getResponse()} \nText: ${itemResponses[5].getResponse()} \nDisplay From: ${itemResponses[6].getResponse()} \nDisplay Until: ${itemResponses[7].getResponse()}`;
  let discordUsername = ''

  let discordUrl = '';
  
  var payload = JSON.stringify({content: discordMessageString, username: discordUsername});
  var params = {
    method: "POST",
    payload: payload,
    muteHttpExceptions: true,
    contentType: "application/json"
    };

  var response = UrlFetchApp.fetch(discordUrl, params);

}
```