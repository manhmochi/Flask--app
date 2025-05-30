# Flask--app
git remote add origin https://github.com/your-username/flask-app.git
git branch -M main
git push -u origin main
from flask import Flask, request, session, redirect, url_for, escape

app = Flask(__name__)
app.secret_key = 'your_secret_key_here'

@app.route('/')
def index():
    if 'username' in session:
        return f'Logged in as {escape(session["username"])}'
    return 'You are not logged in'

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        session['username'] = request.form['username']
        return redirect(url_for('index'))
    return '''
        <form method="post">
            Username: <input type="text" name="username">
            <input type="submit" value="Login">
        </form>
    '''

@app.route('/logout')
def logout():
    session.pop('username', None)
    return redirect(url_for('index'))

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=3000)