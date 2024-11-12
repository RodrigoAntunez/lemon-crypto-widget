async function createWidget() {
    const widget = new ListWidget()
    widget.backgroundColor = new Color("#000000")
    
    try {
        // Obtener precios de Binance
        const symbols = ['BTCUSDT', 'ETHUSDT', 'USDCUSDT']
        const prices = {}
        const changes = {}
        
        // Obtener datos 
        for (const symbol of symbols) {
            try {
                // Obtener precio
                const priceReq = new Request(`https://api.binance.com/api/v3/ticker/price?symbol=${symbol}`)
                priceReq.timeoutInterval = 20
                const priceRes = await priceReq.loadJSON()
                prices[symbol] = parseFloat(priceRes.price)
                
                // Obtener cambio 24h
                const changeReq = new Request(`https://api.binance.com/api/v3/ticker/24hr?symbol=${symbol}`)
                changeReq.timeoutInterval = 20
                const changeRes = await changeReq.loadJSON()
                changes[symbol] = parseFloat(changeRes.priceChangePercent)
                
            } catch (e) {
                console.error(`Error fetching ${symbol}: ${e.message}`)
            }
        }
        
        // Dibujar limón de fondo
        const drawContext = new DrawContext()
        drawContext.size = new Size(320, 160)
        drawContext.opaque = false
        drawContext.setFont(Font.systemFont(60))
        drawContext.setTextColor(new Color("#FFEF00", 0.02))
        drawContext.drawText("🍋", new Point(90, 50))
        widget.backgroundImage = drawContext.getImage()
        
        // Header
        const headerStack = widget.addStack()
        headerStack.layoutHorizontally()
        headerStack.centerAlignContent()
        
        const logoText = headerStack.addText("🍋")
        logoText.font = Font.systemFont(14)
        
        headerStack.addSpacer(4)
        
        const lemonText = headerStack.addText("Lemon")
        lemonText.textColor = new Color("#FFFFFF")
        lemonText.font = Font.boldSystemFont(14)
        
        widget.addSpacer(8)
        
        // Función para añadir precios
        function addCryptoPrice(symbol, nombre, precio, cambio24h) {
            const priceStack = widget.addStack()
            priceStack.layoutHorizontally()
            
            const symbolText = priceStack.addText(`${symbol} ${nombre}`)
            symbolText.textColor = new Color("#FFEF00")
            symbolText.font = Font.systemFont(14)
            
            priceStack.addSpacer()
            
            const priceChangeStack = priceStack.addStack()
            priceChangeStack.layoutHorizontally()
            
            const priceText = priceChangeStack.addText(`$${precio.toLocaleString()}`)
            priceText.textColor = cambio24h >= 0 ? new Color("#00FF88") : new Color("#FF3B30")
            priceText.font = Font.systemFont(14)
            
            if (cambio24h) {
                priceChangeStack.addSpacer(4)
                const changeText = priceChangeStack.addText(`${cambio24h >= 0 ? '↑' : '↓'} ${Math.abs(cambio24h).toFixed(2)}%`)
                changeText.textColor = cambio24h >= 0 ? new Color("#00FF88") : new Color("#FF3B30")
                changeText.font = Font.systemFont(12)
            }
            
            widget.addSpacer(4)
        }
        
        // Añadir precios si están disponibles
        if (prices.BTCUSDT) addCryptoPrice("₿", "BTC", prices.BTCUSDT, changes.BTCUSDT)
        if (prices.ETHUSDT) addCryptoPrice("Ξ", "ETH", prices.ETHUSDT, changes.ETHUSDT)
        if (prices.USDCUSDT) addCryptoPrice("$", "USDC", prices.USDCUSDT, changes.USDCUSDT)
        
    } catch (error) {
        console.error(`Error principal: ${error.message}`)
        
        const errorStack = widget.addStack()
        errorStack.layoutVertically()
        
        const errorTitle = errorStack.addText("⚠️ Error de conexión")
        errorTitle.textColor = new Color("#FF3B30")
        errorTitle.font = Font.boldSystemFont(14)
        
        errorStack.addSpacer(4)
        
        const errorMsg = errorStack.addText("Verifica tu conexión a internet")
        errorMsg.textColor = new Color("#666666")
        errorMsg.font = Font.systemFont(12)
    }
    
    widget.setPadding(12, 16, 12, 16)
    return widget
}

// Ejecutar el widget
let widget = await createWidget()
if (config.runsInWidget) {
    Script.setWidget(widget)
} else {
    widget.presentMedium()
}
Script.complete()
