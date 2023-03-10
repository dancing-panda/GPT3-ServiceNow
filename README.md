# GPT3-ServiceNow
Integration of GPT3 with ServiceNow via Virtual Agent

You can read about GPT3 and ChatGPT here : https://openai.com/

ππ¬ππππ¬π:
Create two topics in Virtual Agent and use ChatGPT for response to the chat:
1. "General Queries"-which will be available to End users to ask any general questions
2. "Developer Support" -which will be available for ServiceNow Developers alone and can support them in Technical queries, Code samples and solutions.

πππ«π ππ«π π­π‘π π¬π­ππ©π¬ :
1. Create a REST message and set the Endpoint toΒ OpenAI- Completions model
   ππππ π¦ππ¬π¬ππ π ππ§ππ©π¨π’π§π­:Β  https://api.openai.com/v1/completions
 
2. Configure Authentication ,Content-Type as specified in OpenAI API Docs.
3. Create a Post -HTTP method and configure the content,variable substitutions accordingly to the OpenAI API requirements.Test Once Complete.
       ``` 
        {
         "model": "${model}",
         "prompt": "${prompt}",
         "max_tokens": ${max_tokens},
         "temperature": ${temperature}
        }
        ```
![HTTP POST method](https://user-images.githubusercontent.com/103847014/213175118-f1317861-3db7-44d5-bb86-8d792f93d422.png)

4. Create two topics "General Queries", "Developer Support" and restrict access based upon your needs
5. InΒ Virtual Agent Designer as a 1st step under "Input Variables" select text and configure it accordingly to your needs
6. InΒ Virtual Agent Designer as a 2nd step under "Bot Response" select script and configure it accordingly to your needs(You can use vaInputs.user_input_field.getDisplayValue()) to pass your message to CHATGPT )

```
ππ¨π« ππ­ππ© 6-"ππ¨π­ πππ¬π©π¨π§π¬π"-πππ«π’π©π­:
(function execute() {
var request = new sn_ws.RESTMessageV2("ChatGPT","askChatGPT");
Β Β Β Β Β Β Β Β var temperature = 0;
Β Β Β Β Β Β Β Β var maxtokens =1024;
Β Β Β Β Β Β Β Β request.setStringParameterNoEscape("model","text-davinci-003");
Β Β Β Β Β Β Β Β request.setStringParameterNoEscape("prompt", vaInputs.user_input.getDisplayValue());
Β Β Β Β Β Β Β Β request.setStringParameterNoEscape("temperature",temperature);
Β Β Β Β Β Β Β Β request.setStringParameterNoEscape("max_tokens",maxtokens);
Β Β Β Β Β Β Β Β var response = request.execute();
Β Β Β Β Β Β Β Β 
Β Β Β Β Β Β Β Β // Parse the response
Β Β Β Β Β Β Β Β var response_body = JSON.parse(response.getBody());
Β Β Β Β Β Β Β Β var answer = response_body.choices[0].text;
        
Β Β Β Β Β Β Β Β //print the response from ChatGPT
Β Β Β Β Β Β Β Β return answer;
})()

```
7. Publish and Test the topics.

**Note:**
setStringParameter() -> to set simple string values such as sys_created_by which won't have special characters
XML reserved characters in the value are converted to the equivalent escaped characters.

setStringParameterNoEscape() -> to set work notes etc where end user can enter special character and in case you want to send that special character as it is without escaping
It does not escape XML reserved characters.

XML reserved characters are: 
> 
<  
&
%  
