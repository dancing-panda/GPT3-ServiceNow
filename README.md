# GPT3-ServiceNow
Integration of GPT3 with ServiceNow via Virtual Agent

You can read about GPT3 and ChatGPT here : https://openai.com/

𝐔𝐬𝐞𝐂𝐚𝐬𝐞:
Create two topics in Virtual Agent and use ChatGPT for response to the chat:
1."General Queries"-which will be available to End users to ask any general questions
2."Developer Support" -which will be available for ServiceNow Developers alone and can support them in Technical queries, Code samples and solutions.

𝐇𝐞𝐫𝐞 𝐚𝐫𝐞 𝐭𝐡𝐞 𝐬𝐭𝐞𝐩𝐬 :
1. Create a REST message and set the Endpoint to OpenAI- Completions model
2.Configure Authentication ,Content-Type as specified in OpenAI API Docs.
3.Create a Post -HTTP method and configure the content,variable substitutions accordingly to the OpenAI API requirements.Test Once Complete.

![HTTP POST method](https://user-images.githubusercontent.com/103847014/213175118-f1317861-3db7-44d5-bb86-8d792f93d422.png)

4.Create two topics "General Queries", "Developer Support" and restrict access based upon your needs5.In Virtual Agent Designer as a 1st step under "Input Variables" select text and configure it accordingly to your needs6.In Virtual Agent Designer as a 2nd step under "Bot Response" select script and configure it accordingly to your needs(You can use vaInputs.user_input_field.getDisplayValue()) to pass your message to CHATGPT )
7.Publish and Test the topics .

𝐅𝐨𝐫 𝐒𝐭𝐞𝐩 1 -𝐑𝐄𝐒𝐓 𝐦𝐞𝐬𝐬𝐚𝐠𝐞 𝐄𝐧𝐝𝐩𝐨𝐢𝐧𝐭:  https://api.openai.com/v1/completions

𝐅𝐨𝐫 𝐒𝐭𝐞𝐩 3 -𝐂𝐨𝐧𝐭𝐞𝐧𝐭:
{
 "model": "${model}",
 "prompt": "${prompt}",
 "max_tokens": ${max_tokens},
 "temperature": ${temperature}
}

𝐅𝐨𝐫 𝐒𝐭𝐞𝐩 6-"𝐁𝐨𝐭 𝐑𝐞𝐬𝐩𝐨𝐧𝐬𝐞"-𝐒𝐜𝐫𝐢𝐩𝐭:
(function execute() {
var request = new sn_ws.RESTMessageV2("ChatGPT","askChatGPT");
        var temperature = 0;
        var maxtokens =1024;
        request.setStringParameterNoEscape("model","text-davinci-003");
        request.setStringParameterNoEscape("prompt", vaInputs.user_input.getDisplayValue());
        request.setStringParameterNoEscape("temperature",temperature);
        request.setStringParameterNoEscape("max_tokens",maxtokens);
        var response = request.execute();
        
        // Parse the response
        var response_body = JSON.parse(response.getBody());
        var answer = response_body.choices[0].text;
        
        //print the response from ChatGPT
        return answer;
})()

Note:
setStringParameter() -> to set simple string values such as sys_created_by which won't have special characters
XML reserved characters in the value are converted to the equivalent escaped characters.

setStringParameterNoEscape() -> to set work notes etc where end user can enter special character and in case you want to send that special character as it is without escaping
It does not escape XML reserved characters.

XML reserved characters are: 
> 
<  
&
%  
