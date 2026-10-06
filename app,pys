from flask import Flask, request, jsonify
import requests

app = Flask(_name_)

# Replace with your actual Page Access Token
PAGE_ACCESS_TOKEN = "YOUR_PAGE_ACCESS_TOKEN"

@app.route("/webhook", methods=["GET", "POST"])
def webhook():
    if request.method == "GET":
        mode = request.args.get("hub.mode")
        token = request.args.get("hub.verify_token")
        challenge = request.args.get("hub.challenge")

        if mode == "subscribe" and token == "YOUR_VERIFY_TOKEN":
            return challenge
        else:
            return "Forbidden", 403

    elif request.method == "POST":
        data = request.json
        if data.get("object") == "page":
            for entry in data.get("entry", []):
                for messaging in entry.get("messaging", []):
                    sender_id = messaging["sender"]["id"]
                    if "message" in messaging:
                        message_text = messaging["message"].get("text")
                        if message_text:
                            reply_text = f"আপনি বলেছেন: {message_text}"
                            send_message(sender_id, reply_text)
            return "EVENT_RECEIVED", 200

def send_message(recipient_id, message_text):
    endpoint = f"https://graph.facebook.com/v12.0/me/messages?access_token={PAGE_ACCESS_TOKEN}"
    payload = {
        "recipient": {"id": recipient_id},
        "message": {"text": message_text},
    }
    response = requests.post(endpoint, json=payload)
    return response.status_code

if _name_ == "_main_":
    app.run(port=5000)
