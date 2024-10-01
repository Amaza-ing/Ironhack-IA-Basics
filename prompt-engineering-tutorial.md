## Prompt engineering best practices

Prompt engineering is all about how to design your prompts so that the response is what you were indeed hoping to see.

The idea of using "unfancy" prompts is to minimize the noise in your prompt to reduce the possibility of the LLM misinterpreting the intent of the prompt. Below are a few guidelines on how to engineer "unfancy" prompts.

In this section, you'll cover the following best practices when engineering prompts:

-   Be concise
-   Be specific, and well-defined
-   Ask one task at a time
-   Improve response quality by including examples
-   Turn generative tasks to classification tasks to improve safety

### Be concise

🛑 Not recommended. The prompt below is unnecessarily verbose.

prompt = "What do you think could be a good name for a flower shop that specializes in selling bouquets of dried flowers more than fresh flowers?"

✅ Recommended. The prompt below is to the point and concise.

prompt = `Suggest a name for a flower shop that sells bouquets of dried flowers`

### Be specific, and well-defined

Suppose that you want to brainstorm creative ways to describe Earth.

🛑 The prompt below might be a bit too generic (which is certainly OK if you'd like to ask a generic question!)

prompt = "Tell me about Earth" 

✅ Recommended. The prompt below is specific and well-defined.

prompt = `Generate a list of ways that makes Earth unique compared to other planets` 

### Ask one task at a time

🛑 Not recommended. The prompt below has two parts to the question that could be asked separately.

prompt = "What's the best method of boiling water and why is the sky blue?" 

✅ Recommended. The prompts below asks one task a time.

prompt = `What's the best method of boiling water?` 

prompt = `Why is the sky blue?`

### Watch out for hallucinations

Although LLMs have been trained on a large amount of data, they can generate text containing statements not grounded in truth or reality; these responses from the LLM are often referred to as "hallucinations" due to their limited memorization capabilities. Note that simply prompting the LLM to provide a citation isn't a fix to this problem, as there are instances of LLMs providing false or inaccurate citations. Dealing with hallucinations is a fundamental challenge of LLMs and an ongoing research area, so it is important to be cognizant that LLMs may seem to give you confident, correct-sounding statements that are in fact incorrect.

Note that if you intend to use LLMs for the creative use cases, hallucinating could actually be quite useful.

Try the prompt like the one below repeatedly. It's possible that it may provide an inaccurate, but confident answer.

prompt = `What day is it today?`

Since LLMs do not have access to real-time information without further integrations, you may have noticed it hallucinates what day it is today in some of the outputs.

### Using system instructions to guardrail the model from irrelevant responses

How can we attempt to reduce the chances of irrelevant responses and hallucinations?

One way is to provide the LLM with  [system instructions](https://cloud.google.com/vertex-ai/generative-ai/docs/multimodal/send-chat-prompts-gemini#system-instructions).

Let's see how system instructions works and how you can use them to reduce hallucinations or irrelevant questions for a travel chatbot.

Suppose we ask a simple question about one of Italy's most famous tourist spots.

 prompt = `Hello! You are an AI chatbot for a travel web site. Your mission is to provide helpful queries for travelers. Remember that before you answer a question, you must check to see if it complies with your mission. If not, you can say, Sorry I can't answer that question.`
 
 prompt = `What is the best place for sightseeing in Milan, Italy?`

Now let us pretend to be a user asks the chatbot a question that is unrelated to travel.

prompt = `What's for dinner?`

You can see that this way, a guardrail in the prompt prevented the chatbot from veering off course.

### Turn generative tasks into classification tasks to reduce output variability

#### Generative tasks lead to higher output variability

The prompt below results in an open-ended response, useful for brainstorming, but response is highly variable.

prompt = `I'm a high school student. Recommend me a programming activity to improve my skills.`

#### Classification tasks reduces output variability

The prompt below results in a choice and may be useful if you want the output to be easier to control.

prompt = `I'm a high school student. Which of these activities do you suggest and why: a) learn Python b) learn JavaScript c) learn Fortran ` 

### Improve response quality by including examples

Another way to improve response quality is to add examples in your prompt. The LLM learns in-context from the examples on how to respond. Typically, one to five examples (shots) are enough to improve the quality of responses. Including too many examples can cause the model to over-fit the data and reduce the quality of responses.

Similar to classical model training, the quality and distribution of the examples is very important. Pick examples that are representative of the scenarios that you need the model to learn, and keep the distribution of the examples (e.g. number of examples per class in the case of classification) aligned with your actual distribution.

#### Zero-shot prompt

Below is an example of zero-shot prompting, where you don't provide any examples to the LLM within the prompt itself.

prompt = `Decide whether a Tweet's sentiment is positive, neutral, or negative. Tweet: I loved the new YouTube video you made! Sentiment: ` 

#### One-shot prompt

Below is an example of one-shot prompting, where you provide one example to the LLM within the prompt to give some guidance on what type of response you want.

prompt = `Decide whether a Tweet's sentiment is positive, neutral, or negative. Tweet: I loved the new YouTube video you made! Sentiment: positive Tweet: That was awful. Super boring 😠 Sentiment: `

#### Few-shot prompt

Below is an example of few-shot prompting, where you provide a few examples to the LLM within the prompt to give some guidance on what type of response you want.

prompt = `Decide whether a Tweet's sentiment is positive, neutral, or negative. Tweet: I loved the new YouTube video you made! Sentiment: positive. Tweet: That was awful. Super boring 😠 Sentiment: negative Tweet: Something surprised me about this video - it was actually original. It was not the same old recycled stuff that I always see. Watch it - you will not regret it. Sentiment: `

#### Choosing between zero-shot, one-shot, few-shot prompting methods

Which prompt technique to use will solely depends on your goal. The zero-shot prompts are more open-ended and can give you creative answers, while one-shot and few-shot prompts teach the model how to behave so you can get more predictable answers that are consistent with the examples provided.
