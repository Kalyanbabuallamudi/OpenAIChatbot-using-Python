import openai
from nltk.chat.util import Chat, reflections

# Set your OpenAI GPT-3 API key
openai.api_key = 'ENTER Your OPEN API CHAT GPT KEY'

# Define the patterns and responses for the rule-based chatbot
rule_based_pairs = [
    [
        r"Hii is (.*)",
        ["Hello %1, how can I help you today?",]
    ],
    [
        r"what is your name?",
        ["I am a chatbot.",]
    ],
    [
        r"how are you?",
        ["I'm doing well, thank you!",]
    ],
    [
        r"(.*) (weather|temperature) in (.*)",
        ["The weather in %3 is great!",]
    ],
    [
        r"(.*) advice",
        ["Sure, my advice is: %1",]
    ],
    [
        r"quit",
        ["Goodbye! Have a great day.",]
    ],
]

# Create a rule-based chatbot using the pairs defined above
rule_based_chatbot = Chat(rule_based_pairs, reflections)

# GPT-3 based chatbot function
def gpt3_chatbot(prompt):
    response = openai.Completion.create(
        engine="text-davinci-002",
        prompt=prompt,
        max_tokens=150
    )
    return response.choices[0].text.strip()

# Start the conversation loop with integrated chatbots
def chat():
    print("Hi, I'm a chatbot. Type 'quit' to exit.")
    while True:
        user_input = input("You: ")
        if user_input.lower() == 'quit':
            print("Chatbot: Goodbye! Have a great day.")
            break

        # Use rule-based chatbot for specific patterns
        rule_based_response = rule_based_chatbot.respond(user_input)

        # Use GPT-3 for general conversation
        gpt3_response = gpt3_chatbot(user_input)

        # Choose the response based on context or confidence
        if "weather" in user_input:
            print("Chatbot:", rule_based_response)
        else:
            print("Chatbot:", gpt3_response)

# Start the chat
if __name__ == "__main__":
    chat()

