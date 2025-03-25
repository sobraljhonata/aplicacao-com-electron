### - Passo 1: Iniciando o projeto

```bash
sudo apt-get install libnss3 libx11-xcb1 libxcomposite1 libxcursor1 libxdamage1 libxi6 libxtst6 libnss3 libxrandr2 libasound2 libatk1.0-0 libpangocairo-1.0-0 libgtk-3-0
ldconfig -p | grep libnss3
ldconfig -p | grep libasound

mkdir electron-example
cd electron-example
npm init -y
npm install electron --save-dev
```
1. **Crie a seguinte estrutura de arquivos estrutura do projeto**
```
electron-example
├── main.js          # Arquivo principal do Electron
├── index.html       # Interface da aplicação
├── renderer.js      # Lógica da interface
```

2. **Insira o seguinte código no arquivo main.js**:
```javascript
const { app, BrowserWindow } = require('electron');

let mainWindow;

app.on('ready', () => {
  mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: __dirname + '/renderer.js', // Preload script
    },
  });

  mainWindow.loadFile('index.html');
  mainWindow.on('closed', () => {
    mainWindow = null;
  });
});

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

app.on('activate', () => {
  if (BrowserWindow.getAllWindows().length === 0) {
    createWindow();
  }
});

```
3. **Insira o seguinte código no arquivo index.html**:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Electron Example</title>
</head>
<body>
  <h1>Bem-vindo ao Electron!</h1>
  <button id="btn">Clique aqui</button>
  <p id="output"></p>
  <script src="renderer.js"></script>
</body>
</html>
```
4. **Insira o seguinte código no arquivo renderer.js**:
```javascript
document.getElementById('btn').addEventListener('click', () => {
    document.getElementById('output').innerText = 'Você clicou no botão!';
  });
```
5. **Dentro do arquivo package.json que foi criado no início do projeto insira dentro da sessão script o comando de iniciar a aplicação**:
```json
...
"scripts": {
    "start": "electron ."
}
...
```

2. **Certifique-se de que na sessão main do arquivo package.json está da seguinte forma**:
```json
...
"main": "main.js",
...
```
```bash
npm start
```