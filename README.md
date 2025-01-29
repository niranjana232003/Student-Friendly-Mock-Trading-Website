# Student-Friendly-Mock-Trading-Website
let balance = 10000;
let portfolio = [];
const stocks = [
    { name: "AAPL", price: 150, img: "https://api.time.com/wp-content/uploads/2015/02/applelogo.jpg" },
    { name: "GOOG", price: 2800, img: "https://th.bing.com/th/id/OIP.S3ZsU5iH6e3Z2K7lXlES7AHaFj?rs=1&pid=ImgDetMain" },
    { name: "TSLA", price: 750, img: "https://th.bing.com/th/id/OIP.ulK8N06MeQax0JzkXEY1QHaEM?rs=1&pid=ImgDetMain" },
];

function navigate(page) {
    document.querySelectorAll("main > section").forEach((section) => {
        section.style.display = "none";
    });
    document.getElementById(page).style.display = "block";

    if (page === "dashboard") displayStocks();
    if (page === "portfolio") displayPortfolio();
}

function displayStocks() {
    const stocksDiv = document.getElementById("stocks");
    stocksDiv.innerHTML = "";
    stocks.forEach((stock, index) => {
        const stockDiv = document.createElement("div");
        stockDiv.className = "stock";
        stockDiv.innerHTML = `
            <img src="${stock.img}" alt="${stock.name}">
            <p>${stock.name}: $${stock.price}</p>
            <button onclick="buyStock(${index})">Buy</button>
            <button onclick="sellStock(${index})">Sell</button>
        `;
        stocksDiv.appendChild(stockDiv);
    });
}

function buyStock(index) {
    const stock = stocks[index];
    if (balance >= stock.price) {
        balance -= stock.price;
        const ownedStock = portfolio.find((item) => item.name === stock.name);
        if (ownedStock) {
            ownedStock.quantity++;
        } else {
            portfolio.push({ name: stock.name, price: stock.price, quantity: 1, img: stock.img });
        }
        updateBalance();
    } else {
        alert("Insufficient balance!");
    }
}

function sellStock(index) {
    const stock = stocks[index];
    const ownedStock = portfolio.find((item) => item.name === stock.name);
    if (ownedStock && ownedStock.quantity > 0) {
        balance += stock.price;
        ownedStock.quantity--;
        if (ownedStock.quantity === 0) {
            portfolio = portfolio.filter((item) => item.name !== stock.name);
        }
        updateBalance();
    } else {
        alert("You don't own this stock!");
    }
}

function displayPortfolio() {
    const portfolioDiv = document.getElementById("portfolio-list");
    portfolioDiv.innerHTML = portfolio.length
        ? portfolio
              .map(
                  (stock) =>
                      `<div class="portfolio-stock">
                          <img src="${stock.img}" alt="${stock.name}">
                          <p>${stock.name} - Quantity: ${stock.quantity} - Value: $${stock.quantity * stock.price}</p>
                      </div>`
              )
              .join("")
        : "<p>No stocks owned yet!</p>";
}

function updateBalance() {
    document.getElementById("balance").textContent = balance;
}
