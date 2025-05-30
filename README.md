<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Menú Restaurante</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      background: linear-gradient(135deg, #f8f9fa, #e0f7fa);
      color: #333;
    }
    h1, h2 {
      text-align: center;
      color: #2c3e50;
    }
    .menu-item {
      background: #fff;
      padding: 15px;
      margin-bottom: 10px;
      border-radius: 8px;
      box-shadow: 0 0 5px rgba(0,0,0,0.1);
    }
    .precio {
      font-weight: bold;
      color: green;
    }
    .boton {
      display: inline-block;
      margin: 10px auto;
      padding: 10px 20px;
      background: #28a745;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      text-align: center;
    }
    .contador {
      margin-top: 10px;
      font-weight: bold;
    }
    .link-pago {
      display: block;
      margin: 10px auto;
      padding: 10px;
      background: #007bff;
      color: white;
      text-decoration: none;
      border-radius: 5px;
      max-width: 300px;
    }
    #mediosPago, #total, #extras {
      display: none;
      text-align: center;
      margin-top: 30px;
      font-weight: bold;
    }
    .categoria {
      background-color: #ddd;
      padding: 10px;
      border-radius: 5px;
      margin-top: 20px;
      font-weight: bold;
    }
    input, select, textarea {
      display: block;
      margin: 10px auto;
      padding: 8px;
      width: 90%;
      max-width: 500px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    a#WHATSAPP {
      display: inline-block;
      margin-top: 10px;
      padding: 10px;
      background-color: #25D366;
      color: white;
      border-radius: 5px;
      text-decoration: none;
    }
  </style>
</head>
<body>
  <h1>Menú Restaurante</h1>
  <div id="menu"></div>
  <div id="total">Total a pagar: $<span id="totalValor">0</span></div>
  <button class="boton" onclick="finalizarCompra()">Finalizar Compra</button>
  <button class="boton" onclick="reiniciar()">Reiniciar</button>

  <div id="extras">
    <h2>Datos de Entrega</h2>
    <input type="text" id="direccion" placeholder="Escribe la dirección..." />
    <textarea id="comentario" placeholder="Descripción / Comentario"></textarea>
    <select id="zona">
      <option value="">Selecciona zona de domicilio</option>
      <option value="4000">Cerca ($4.000)</option>
      <option value="5000">Lejos ($5.000)</option>
      <option value="6000">Más lejos de Funza ($6.000)</option>
      <option value="7000">Muy lejos ($7.000)</option>
    </select>
    <a id="WHATSAPP" target="_blank">Enviar por WhatsApp</a>
  </div>

  <div id="mediosPago">
    <h2>Medios de Pago</h2>
    <a href="intent://send?phone=+573152553101#Intent;scheme=nequi;package=com.nequi.mobile.app;end" class="link-pago">Pagar con Nequi</a>
    <a href="intent://send?phone=+573152553101#Intent;scheme=daviplata;package=com.davivienda.daviplata;end" class="link-pago">Pagar con Daviplata</a>
    <p>📞 Número: <strong>3152553101</strong></p>
  </div>

  <script>
    const menu = [
        { categoria: "Hamburguesas", nombre: "Hamburguesa Sencilla", descripcion: "Carne artesanal(100g), queso doble crema, verduras, salsas, pan artesanal.", precio: 10000 },
      { categoria: "Hamburguesas", nombre: "Hamburguesa De Patakon", descripcion: "Carne artesanal(100g), queso, verduras, salsas, patacón maduro.", precio: 13000 },
      { categoria: "Hamburguesas", nombre: "Hamburguesa Especial", descripcion: "Carne(150g), tocineta, huevo, queso, verduras, salsas, pan.", precio: 15000 },
      { categoria: "Hamburguesas", nombre: "Hamburguesa Tetakeo", descripcion: "Carne y pechuga (150g c/u), tocineta, huevo, verduras, pan.", precio: 22000 },
      { categoria: "Perros Calientes", nombre: "Perro Caliente", descripcion: "Salchicha americana, cebolla, papas, queso, salsas, pan.", precio: 8000 },
      { categoria: "Perros Calientes", nombre: "Perro Especial Mechiperro", descripcion: "Salchicha, carne esmechada, papas, queso, verduras, pan.", precio: 15000 },
      { categoria: "Salchipapas", nombre: "Salchipapa Sencilla", descripcion: "Salchicha, papas, queso, salsas.", precio: 10500 },
      { categoria: "Salchipapas", nombre: "Salchipapa Especial", descripcion: "Carne esmechada, salchicha, papas, verduras, huevo codorniz.", precio: 20500 },
      { categoria: "Salchipapas", nombre: "Salchipapa Especial para dos", descripcion: "Carne, salchichas, papas dobles, verduras, huevo codorniz.", precio: 26500 },
      { categoria: "Salchipapas", nombre: "Salchipapa Especial de Pollo", descripcion: "Pollo, carne, salchicha, papas, verduras, huevo codorniz.", precio: 23500 },
      { categoria: "Otros", nombre: "Picada", descripcion: "Carnes surtidas, papas, verduras, salsas, huevo codorniz.", precio: 38500 },
      { categoria: "Otros", nombre: "Mazorca", descripcion: "Carnes, papas, queso, maíz, salsas.", precio: 38500 },
      { categoria: "Otros", nombre: "Papas a la Francesa (Porción)", descripcion: "Porción de papas a la francesa", precio: 7000 },
      { categoria: "Bebidas", nombre: "Bebida Coca-Cola", descripcion: "Coca-Cola personal 500ml", precio: 4000 },
      { categoria: "Bebidas", nombre: "Bebida Sprite", descripcion: "Sprite personal 400ml", precio: 4000 },
      { categoria: "Bebidas", nombre: "Bebida Cuatro", descripcion: "Cuatro personal 400ml", precio: 4000 },
      { categoria: "Bebidas", nombre: "Bebida Kola Román", descripcion: "Kola Román personal 400ml", precio: 4000 },
      { categoria: "Combos", nombre:"Combo Sencillo", descripcion: "Hamburguesa sencilla + bebida + papas.", precio: 20000 },
      { categoria: "Combos", nombre:"Combo Especial", descripcion: "Hamburguesa especial + bebida + papas.", precio: 26000 },
      { categoria: "Combos", nombre:"Combo Tetakeo", descripcion: "Hamburguesa Tetakeo + bebida + papas.", precio: 30000 },
      { categoria: "Promociones", nombre: "Promo Martes: 2 Hamburguesas Sencillas", descripcion: "2 Hamburguesas por $18.000", precio: 18000 },
      { categoria: "Promociones", nombre: "Promo Martes: 2 Perros Calientes", descripcion: "2 Perros Calientes por $12.000", precio: 12000 },
      { categoria: "Promociones", nombre: "Promo Martes: Mazorcada Sencilla", descripcion: "Mazorcada sencilla por $22.500", precio: 22500 }
    ];

    let total = 0;
    let cantidades = new Array(menu.length).fill(0);
    const contenedorMenu = document.getElementById("menu");

    let categoriaActual = "";
    menu.forEach((item, index) => {
      if (item.categoria !== categoriaActual) {
        const cat = document.createElement("div");
        cat.className = "categoria";
        cat.textContent = item.categoria;
        contenedorMenu.appendChild(cat);
        categoriaActual = item.categoria;
      }
      const div = document.createElement("div");
      div.className = "menu-item";
      div.innerHTML = `
        <h3>${item.nombre}</h3>
        <p>${item.descripcion}</p>
        <p class='precio'>$${item.precio.toLocaleString()}</p>
        <p class='contador'>Cantidad: <span id="cantidad-${index}">0</span></p>
        <button onclick="agregar(${item.precio}, ${index})">Agregar</button>
        <button onclick="quitar(${item.precio}, ${index})">Quitar</button>
      `;
      contenedorMenu.appendChild(div);
    });

    function actualizarTotal() {
      document.getElementById("totalValor").textContent = total.toLocaleString();
    }

    function agregar(precio, index) {
      total += precio;
      cantidades[index]++;
      document.getElementById(`cantidad-${index}`).textContent = cantidades[index];
      document.getElementById("total").style.display = "block";
      actualizarTotal();
    }

    function quitar(precio, index) {
      if (cantidades[index] > 0) {
        total -= precio;
        cantidades[index]--;
        document.getElementById(`cantidad-${index}`).textContent = cantidades[index];
        actualizarTotal();
      }
    }

    function finalizarCompra() {
      document.getElementById("extras").style.display = "block";
      document.getElementById("mediosPago").style.display = "block";
      enviarPorWhatsApp();
      window.scrollTo(0, document.body.scrollHeight);
    }

    function enviarPorWhatsApp() {
      const direccion = document.getElementById("direccion").value.trim();
      const comentario = document.getElementById("comentario").value.trim();
      const zona = parseInt(document.getElementById("zona").value || 0);
      let totalFinal = total + zona;

      let mensaje = "*📝 Pedido desde el Menú:*%0A%0A";
      menu.forEach((item, i) => {
        if (cantidades[i] > 0) {
          mensaje += `🍔 ${item.nombre} x${cantidades[i]} - $${(item.precio * cantidades[i]).toLocaleString()}%0A`;
        }
      });

      mensaje += `%0A🚚 *Domicilio:* $${zona.toLocaleString()}`;
      mensaje += `%0A💰 *Total a pagar:* $${totalFinal.toLocaleString()}%0A`;

      if (direccion) {
        const linkMaps = `https://www.google.com/maps/search/${encodeURIComponent(direccion)}`;
        mensaje += `%0A📍 *Dirección:* ${direccion}%0A🔗 ${linkMaps}`;
      }

      if (comentario) {
        mensaje += `%0A🗒️ *Comentario:* ${comentario}`;
      }

      mensaje += `%0A%0A✅ Por favor confirmar disponibilidad.`;

      const telefono = "3152553101";
      const url = `https://wa.me/${telefono}?text=${encodeURIComponent(mensaje)}`;
      document.getElementById("WHATSAPP").href = url;
    }

    function reiniciar() {
      total = 0;
      cantidades.fill(0);
      menu.forEach((_, i) => {
        document.getElementById(`cantidad-${i}`).textContent = 0;
      });
      actualizarTotal();
      document.getElementById("total").style.display = "none";
      document.getElementById("extras").style.display = "none";
      document.getElementById("mediosPago").style.display = "none";
    }
  </script>
</body>
</html>
