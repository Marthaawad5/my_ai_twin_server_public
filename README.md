# my_ai_twin_server
AI-powered server for my AI twin project
# Install required libraries
!pip install openai python-docx --quiet

from openai import OpenAI
from docx import Document

# Enter your API key here
api_key = input("Enter your OpenAI API key: ")
client = OpenAI(api_key=api_key)

# Create a Word document
doc = Document()
doc.add_heading('Chat History with LLM', level=1)

print("Welcome! Type any question (or type quit to exit).")

while True:
    user_input = input("You: ")

    if user_input.lower() in ["quit", "exit"]:
        doc.save("chat_history.docx")
        print("👋 Goodbye! Your chat has been saved in chat_history.docx")
        break

    # Send the question to the LLM
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[{"role": "user", "content": user_input}]
    )

    bot_reply = response.choices[0].message.content
    print("🤖 LLM:", bot_reply)

    # Add the conversation to the Word file
    doc.add_paragraph(f"You: {user_input}")
    doc.add_paragraph(f"LLM: {bot_reply}")
    doc.add_paragraph("-" * 40)
