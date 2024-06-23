# ChatApp  #

A small functional person-to-person message center application built using Django.
It has a REST API and uses WebSockets to notify clients of new messages and 
avoid polling.

## Architecture ##
- Upon user login, the frontend retrieves the user list and establishes a WebSocket connection to the server for notifications.
- When a user selects another chat participant, the frontend fetches the most recent 15 messages exchanged between them.
- When a user sends a message, the frontend makes a POST request to the REST API, which then saves the message in Django and notifies the relevant users via the WebSocket connection with the new message ID.
- When the frontend gets a new message notification with the message ID, it sends a GET request to the API to retrieve the message

## Scaling ##


**update 04/06/19**

- using pipenv for package management
- move to Channels 2
- use redis as the channel layer backing store. for more information, please check [channels_redis](https://github.com/django/channels_redis)

### Database ###
For this demo, I'm using a simple MySQL setup. If more performance is required, 
a MySQL cluster / shard could be deployed.

PD: I'm using indexes to improve performance.

## Assumptions ##
Because of time constraints this project lacks of:

- User Sign-In / Forgot Password
- User Selector Pagination
- Good Test Coverage
- Better Comments / Documentation Strings
- Frontend Tests
- Modern Frontend Framework (like React)
- Frontend Package (automatic lintin, building and minification)
- Proper UX / UI design (looks plain bootstrap)

## Run ##

0. move to project root folder


1. Create and activate a virtualenv (Python 3)
```bash
pipenv --python 3 shell
```
2. Install requirements
```bash
pipenv install
```
3. Create a MySQL database
```sql
CREATE DATABASE chat CHARACTER SET utf8;
```
4. Start Redis Server
```bash
redis-server
```

5. Init database
```bash
./manage.py migrate
```
6. Run tests
```bash
./manage.py test
```

7. Create admin user
```bash
./manage.py createsuperuser
```

8. Run development server
```bash
./manage.py runserver
```

To override default settings, create a local_settings.py file in the chat folder.

Message prefetch config (load last n messages):
```python
MESSAGES_TO_LOAD = 15
```
