import { createReadStream } from 'node:fs';

const readStream = createReadStream('example.txt', { encoding: 'utf8' });

readStream.on('data', (chunk) => {
    console.log('Novo chunk recebido:', chunk);
});

readStream.on('end', () => {
    console.log('Leitura do arquivo completa.');
});

readStream.on('error', (err) => {
    console.error('Erro ao ler o arquivo:', err);
});
