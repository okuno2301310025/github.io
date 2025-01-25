const express = require('express');
const axios = require('axios');

const app = express();
const PORT = 30147; // サーバのポート番号

app.get('/', (req, res) => {
    res.send(`
        <h1>QR Code Generator</h1>
        <form action="/generate" method="GET">
            <label for="data">QRコードに変換したい文字列を入力してください:</label>
            <input type="text" id="data" name="data" required>
            <button type="submit">QRコード生成</button>
        </form>
    `);
});

app.get('/generate', async (req, res) => {
    const { data } = req.query;

    if (!data) {
        return res.status(400).send('データが入力されていません。');
    }

    const apiUrl = `https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=${encodeURIComponent(data)}`;

    try {
        // APIのURLを直接クライアントに送信
        res.send(`
            <h1>QRコード生成結果</h1>
            <p>生成されたQRコード:</p>
            <img src="${apiUrl}" alt="Generated QR Code" />
            <br />
            <a href="/">戻る</a>
        `);
    } catch (error) {
        console.error(error);
        res.status(500).send('エラーが発生しました。');
    }
});

app.listen(PORT, () => {
    console.log(`サーバが起動しました: http://localhost:${PORT}`);
});
