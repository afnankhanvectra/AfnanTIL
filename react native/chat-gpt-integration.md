# Chat GPT

## Introduction: 

GPT-3 (Generative Pretrained Transformer 3) is a state-of-the-art language processing AI model developed by OpenAI. It is capable of generating human-like text and has a wide range of applications, including language translation, language modeling, and generating text for applications such as chatbots. It can be used on https://chat.openai.com/chat after making an account. In this tutorial, we will find out how can we integrate the chatgpt model in our React native app.

## Integrate chat GPT-3 Rest API on React project

For integration, first, we need to create a new API key that will be used in our application. For that purpose 

1. Go to [openai.com](https://openai.com)
2. Click on the API button
3. Sign up there
4. Click on personal
5. Click on API keys from the left menu
6. Click on generate key and copy generated key

## Implement as Rest API

Now the key is ready and we can implement Rest API for ChatGPt. Create a react native project and open it in the visual studio.

 1. Open Ap. js file and create some UI to make a tutorial.  I am making a simple screen with user input, submit button and an answer label. Here is the samlple UI text.
 
	Copy and replace code in the App.js file render function
	
		return (
    		<View style={styles.container}>
           <SafeAreaView>
		        <TextInput
		          style={styles.input}
		          onChangeText={onChangeText}
		          placeholder="write your question"
		          value={text}
		        />
		        <Button
		          title="Submit"
		          onPress={onSubmitQuestion}
		          style={styles.button}
		        />
		        <Text style={styles.answerText}>{answer}</Text>
		      </SafeAreaView>
		    </View>
		  );

2. Give some style to UI
	
		const styles = StyleSheet.create({
		  container: {
		    flex: 1,
		    backgroundColor: "#fff",
		    alignItems: "center",
		    justifyContent: "center",
		  },
		  input: {
		    height: 40,
		    width: 300,
		    margin: 12,
		    borderWidth: 1,
		    padding: 10,
		  },
		  button: {
		    height: 40,
		    width: 40,
		    margin: 12,
		    padding: 10,
		  },
		  answerText: {
		    margin: 12,
		    padding: 10,
		    textAlign: "center",
		  },
		});
		
3. This function uses 1 state when the text gets changed and one function when the submit button gets called.

        const [text, onChangeText] = useState("");

4. Now implement on submit call

		const onSubmitQuestion = () => {
    	  callAPI(text);
        };
        
  submit button is calling another function to cal API. In this function, you can apply other business logic too

5. Implement CallAPI function to post your query and get results. For that purpose, we will use Fetch.  First, make a request

			const requestOptions = {
		      method: "POST",
		      headers: {
		        "Content-Type": "application/json",
		        Accept: "application/json",
		        Authorization:
		          "Bearer $”{API_KEY_What_we_get from_porrtal },
		      },
			 body: JSON.stringify({
			        model: "text-ada-001",
			        prompt: textToSend,
			        temperature: 0.6,
			        max_tokens: 150,
			      }),
			    };

##### Parameters description: 
* **Authorization:** It has the API Key that we got from OpenAI portal
* **model:** Currently chat-gpt is providing 4 models, Devinci , ADA,curie, and Babbage
* **Max_token:** Limit of answer. Remember this limit is also included question too
* **temperature:** Limit of relevancy  1 means closer 0 means more generalize the solution

## Difference of modal and result
As per [OpenAI](https://beta.openai.com/docs/models)
	
* **text-davinci-003:** Most capable GPT-3 model. Can do any task the other models can do, often with higher quality, longer output and better instruction-following. Also supports inserting completions within text.
* **text-curie-001:**	Very capable, but faster and lower cost than Davinci.
* **text-babbage-001:**	Capable of straightforward tasks, very fast, and lower cost.
* **text-ada-001:** Capable of very simple tasks, usually the fastest model in the GPT-3 series, and lowest cost.

### playground URL
[Chat Gpt Playground](https://beta.openai.com/playground/p/default-vr-fitness?lang=node.js&model=text-ada-001)


