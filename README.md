# project-server-ilyas

const http = require('http');

const motoGP = [
    {
        circuit: 'Losail',
        location: 'Qatar',
        winner: {
            firstName: 'Andrea',
            lastName: 'Dovizioso',
            country: 'Italy'
        }
    },
    {
        circuit: 'Autodromo',
        location: 'Argentine',
        winner: {
            firstName: 'Cal',
            lastName: 'Crutchlow',
            country: 'UK'
        }
    },
    {
        circuit: 'De Jerez',
        location: 'Spain',
        winner: {
            firstName: 'Valentino',
            lastName: 'Rossi',
            country: 'Italy'
        }
    },
    {
        circuit: 'Mugello',
        location: 'Italy',
        winner: {
            firstName: 'Andrea',
            lastName: 'Dovizioso',
            country: 'Italy'
        }
    }
];

const server = http.createServer((req, res) => {
    res.setHeader('Content-Type', 'application/json');
    const url = req.url;

    if (url === '/') {
        res.statusCode = 200; // OK
        res.end(JSON.stringify(motoGP));

    } else if (url === '/country') {
        const groupedByCountry = motoGP.reduce((acc, race) => {
            const country = race.winner.country;
            if (!acc[country]) {
                acc[country] = []; 
            }
            acc[country].push(race); 
            return acc;
        }, {});

        res.statusCode = 200;
        res.end(JSON.stringify(groupedByCountry));

    } else if (url === '/name') {
        
        const groupedByName = motoGP.reduce((acc, race) => {
            const winnerName = `${race.winner.firstName} ${race.winner.lastName}`;
            if (!acc[winnerName]) {
                acc[winnerName] = []; 
            }
            acc[winnerName].push(race); 
            return acc;
        }, {});

        res.statusCode = 200; 
        res.end(JSON.stringify(groupedByName));

    } else {
        
        res.statusCode = 400; 
        res.setHeader('Content-Type', 'text/plain'); 
        res.end('Bad Request');
    }
});

const port = 8000;
const host = 'localhost';

server.listen(port, host, () => {
    console.log(`Server berjalan di http://${host}:${port}`);
});
