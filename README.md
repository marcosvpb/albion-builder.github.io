<!DOCTYPE html>
<html lang="pt-BR">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Albion Build Maker</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/notify/0.4.2/notify.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Ubuntu:wght@400;500;700&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
    <link rel="icon" type="image/png" href="https://iili.io/fVRrpxp.md.png">


    <style>
        html,
        body {
            height: 100%;
            font-family: 'Ubuntu', sans-serif;
            color: white;
            background:
                linear-gradient(rgba(0, 0, 0, 0.55), rgba(0, 0, 0, 0.55)),
                url("https://assets.albiononline.com/uploads/media/default/media/03f4ae95f6a2ad7ced5ea3386e70dd403b3a4276.jpeg") center / cover no-repeat fixed;
        }

        .image-container {
            height: 5em;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .image-container img {
            max-height: 120%;
            object-fit: contain;
            top: -15px;

        }

        .container {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 5px;
        }

        .main-items {
            display: grid;
            gap: 4px;
        }

        .scroll-box {
            min-height: 6vh;
            max-height: 20vh;
            overflow-y: auto;
            border: 3px solid #ffae00;
            border-radius: 5px;
            padding: 10px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            padding: 10px;
        }

        .scroll-box-item {
            max-height: 90vh;
            overflow: auto;
            border: 3px solid #ffae00;
            border-radius: 5px;
            padding: 10px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            padding: 5px;
        }


        .scroll-box img {
            display: block;
            margin-bottom: 5px;
            max-width: 100%;
            user-select: none;
        }



        @media (max-width:900px) {
            .container {
                grid-template-columns: repeat(2, 1fr);
            }
        }

        @media (max-width:600px) {
            .container {
                grid-template-columns: 1fr;
            }
        }

        .image-block {
            position: relative;
            background: rgba(30, 30, 30, 0.9);
            border: 1px solid #444;
            padding: 4px;
            border-radius: 6px;
            backdrop-filter: blur(3px);
        }

        .image-block.dragging {
            opacity: 0.4;
        }

        .image-block.drag-over {
            outline: 2px dashed #2ecc71;
        }

        .images {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: center;
            gap: 2px;
            border: 1px dashed rgba(255, 255, 255, 0.3);
            padding: 2px;
            min-height: 56px;
            max-height: 168px;
        }

        .images-hide {
            display: flex;
            flex-wrap: wrap;
            gap: 4px;
            border: 1px dashed rgba(255, 255, 255, 0.3);
            padding: 8px;
        }


        .images img {
            width: 56px;
            height: 56px;
            cursor: pointer;
        }

        .images img:hover {
            transform: scale(1.45);
            opacity: 0.8;
        }

        button {
            cursor: pointer;
        }

        .btn-main {
            padding: 8px 12px;
            background: #dadada;
            border: none;
            color: #000;
            font-weight: bold;
            border-radius: 4px;
            margin-bottom: 5px;
            margin-top: 5px;
        }

        .footer {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            text-align: center;
            padding: 8px 0;
            font-size: 14px;
            background: #111;
            color: #ccc;
            border-top: 1px solid #333;
        }

        .btn-color {
            padding: 4px 12px;
            background: #dadada;
            border: none;
            color: #000;
            font-weight: bold;
            border-radius: 4px;
            margin-bottom: 5px;
            margin-top: 5px;

        }

        .remove-block {
            position: absolute;
            top: 5px;
            right: 5px;
            background: #e74c3c;
            border: none;
            color: #fff;
            width: 22px;
            height: 22px;
            border-radius: 50%;
            font-weight: bold;
        }

        .btn-add-img {
            margin-top: 8px;
            padding: 5px 8px;
            background: #3498db;
            border: none;
            color: #fff;
            border-radius: 4px;
            font-size: 12px;
        }

        .hide-ui button {
            display: none !important;
        }

        .hide-ui .scroll-box {
            display: none !important;
        }

        .hide-ui input {
            display: none !important;
        }

        .hide-ui .btn-toggle {
            display: inline-block !important;
        }

        #helpBtn {
            position: fixed;
            top: 5px;
            right: 20px;
            width: 44px;
            height: 44px;
            border-radius: 50%;
            border: none;
            background: #4b66a3;
            color: #fff;
            font-size: 22px;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 4px 12px rgba(0, 0, 0, .4);
            z-index: 9999;
        }

        #helpBtn:hover {
            background: #3a3f4a;
        }
    </style>
</head>

<body>
    <h1>
        <!-- <div class="image-container">
            <img src="https://iili.io/fVzH95X.png" alt="Imagem">
        </div> -->

    </h1>

    <!-- Botoes -->
    <button class="btn-main btn-toggle" onclick="hideAll()">Esconder</button>
    <button class="btn-main btn-toggle" onclick="showAll()">Mostrar</button>

    <button class="btn-main btn-toggle" onclick="addBlock()"> Adicionar Build</button>
    <button class="btn-main btn-toggle" onclick="copyBuildUrl()">Copiar link da build</button>

    <button class="btn-main btn-toggle" id="helpBtn" title="Ajuda">?</button>
    <input type="hidden" id="hashInput" style="width:100%; padding:8px; border-radius:4px; border:none;" />
    <input type="text" id="filterInput" placeholder="Filtrar (ex: arcano)"
        style="height: 24px; width: 500px; margin-left: 10px;" oninput="filterBlocks(this.value)" autocomplete="off" />



    <!-- Items -->
    <div class="scroll-box" id="scrollBox">
        <!-- Pano -->
        <div class="main-items" style="position: relative;" data-category="pano">
            <!-- Cabeca -->
            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_CLOTH_ROYAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_CLOTH_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_CLOTH_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_CLOTH_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_CLOTH_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_CLOTH_SET3.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_CLOTH_SET2.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_CLOTH_SET1.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_CLOTH_FEY.png?count=1&amp;quality=1"
                    draggable="true">
                <!-- Armadura -->
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_CLOTH_ROYAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_CLOTH_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_CLOTH_FEY.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_CLOTH_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_CLOTH_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_CLOTH_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_CLOTH_SET3.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_CLOTH_SET2.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_CLOTH_SET1.png?count=1&amp;quality=1"
                    draggable="true">
                <!-- Botas -->
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_CLOTH_ROYAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_CLOTH_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_CLOTH_FEY.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_CLOTH_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_CLOTH_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_CLOTH_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_CLOTH_SET3.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_CLOTH_SET2.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_CLOTH_SET1.png?count=1&amp;quality=1"
                    draggable="true">
            </div>
        </div>

        <!-- Couro -->
        <div class="main-items" style="position: relative;" data-category="couro">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_LEATHER_SET3.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_LEATHER_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_LEATHER_SET1.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_LEATHER_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_LEATHER_SET2.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_LEATHER_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_LEATHER_FEY.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_LEATHER_ROYAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_LEATHER_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <!-- Peito -->
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_LEATHER_ROYAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_LEATHER_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_LEATHER_FEY.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_LEATHER_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_LEATHER_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_LEATHER_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_LEATHER_SET3.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_LEATHER_SET2.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_LEATHER_SET1.png?count=1&amp;quality=1"
                    draggable="true">
                <!-- Botas -->
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_LEATHER_ROYAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_LEATHER_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_LEATHER_FEY.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_LEATHER_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_LEATHER_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_LEATHER_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_LEATHER_SET3.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_LEATHER_SET2.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_LEATHER_SET1.png?count=1&amp;quality=1"
                    draggable="true">

            </div>
        </div>

        <!-- Placa -->
        <div class="main-items" style="position: relative;" data-category="placa">

            <div class="images">

                <!-- Cabeca -->
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_PLATE_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_PLATE_FEY.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_PLATE_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_PLATE_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_PLATE_ROYAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_PLATE_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_PLATE_SET3.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_PLATE_SET2.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_HEAD_PLATE_SET1.png?count=1&amp;quality=1"
                    draggable="true">
                <!-- Peito -->
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_PLATE_ROYAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_PLATE_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_PLATE_FEY.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_PLATE_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_PLATE_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_PLATE_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_PLATE_SET3.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_PLATE_SET2.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_ARMOR_PLATE_SET1.png?count=1&amp;quality=1"
                    draggable="true">
                <!-- Botas -->
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_PLATE_ROYAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_PLATE_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_PLATE_FEY.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_PLATE_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_PLATE_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_PLATE_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_PLATE_SET3.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_PLATE_SET2.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_SHOES_PLATE_SET1.png?count=1&amp;quality=1"
                    draggable="true">
            </div>
        </div>

        <!-- Espadas -->
        <div class="main-items" style="position: relative;" data-category="espada">

            <div class="images">

                <img src="https://render.albiononline.com/v1/item/T8_2H_CLAYMORE.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_CLAYMORE_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DUALSWORD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_SWORD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_SWORD_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DUALSCIMITAR_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_CLEAVER_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_SCIMITAR_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">

            </div>

        </div>

        <!-- Luvas -->
        <div class="main-items" style="position: relative;" data-category="luvas">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_2H_KNUCKLES_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_KNUCKLES_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_KNUCKLES_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_KNUCKLES_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_KNUCKLES_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_KNUCKLES_SET3.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_KNUCKLES_SET2.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_KNUCKLES_SET1.png?count=1&amp;quality=1"
                    draggable="true">

            </div>

        </div>

        <!-- Sagrados -->
        <div class="main-items" style="position: relative;" data-category="sagrados">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_HOLYSTAFF_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_HOLYSTAFF_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_HOLYSTAFF_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_HOLYSTAFF_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_HOLYSTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_HOLYSTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_HOLYSTAFF_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DIVINESTAFF.png?count=1&amp;quality=1"
                    draggable="true">

            </div>
        </div>

        <!-- Natureza -->
        <div class="main-items" style="position: relative;" data-category="naturezas">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_NATURESTAFF_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_NATURESTAFF_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_NATURESTAFF_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_NATURESTAFF_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_NATURESTAFF_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_NATURESTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_NATURESTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_WILDSTAFF.png?count=1&amp;quality=1"
                    draggable="true">

            </div>
        </div>

        <!-- Arcano -->
        <div class="main-items" style="position: relative;" data-category="arcanos">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_2H_ARCANESTAFF_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_ARCANE_RINGPAIR_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_ARCANESTAFF_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_ARCANESTAFF_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_ARCANESTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_ARCANESTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_ENIGMATICORB_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_ENIGMATICSTAFF.png?count=1&amp;quality=1"
                    draggable="true">

            </div>
        </div>

        <!-- Machados -->
        <div class="main-items" style="position: relative;" data-category="machados">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_2H_AXE_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_AXE.png?count=1&amp;quality=1" draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_AXE.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DUALAXE_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_SCYTHE_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_HALBERD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_HALBERD_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_SCYTHE_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">

            </div>
        </div>

        <!-- Adagas -->
        <div class="main-items" style="position: relative;" data-category="adagas">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DAGGERPAIR_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DAGGER_KATAR_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_DAGGER_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DAGGERPAIR.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_DAGGER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DUALSICKLE_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_RAPIER_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_CLAWPAIR.png?count=1&amp;quality=1"
                    draggable="true">

            </div>
        </div>


        <!-- Maças -->
        <div class="main-items" style="position: relative;" data-category="maças">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_2H_FLAIL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_MACE_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_MACE_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_MACE.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_MACE.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_MACE_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DUALMACE_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_ROCKMACE_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">

            </div>
        </div>

        <!-- Martelo -->
        <div class="main-items" style="position: relative;" data-category="martelos">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_2H_HAMMER_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_HAMMER_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_HAMMER_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_HAMMER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_HAMMER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_RAM_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DUALHAMMER_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_POLEHAMMER.png?count=1&amp;quality=1"
                    draggable="true">

            </div>
        </div>


        <!-- Shaper -->
        <div class="main-items" style="position: relative;" data-category="metamorfos">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_2H_SHAPESHIFTER_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_SHAPESHIFTER_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_SHAPESHIFTER_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_SHAPESHIFTER_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_SHAPESHIFTER_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_SHAPESHIFTER_SET3.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_SHAPESHIFTER_SET2.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_SHAPESHIFTER_SET1.png?count=1&amp;quality=1"
                    draggable="true">
            </div>
        </div>


        <!-- Gelo -->
        <div class="main-items" style="position: relative;" data-category="gelados">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_2H_ICECRYSTAL_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_ICEGAUNTLETS_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_FROSTSTAFF_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_FROSTSTAFF_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_FROSTSTAFF_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_FROSTSTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_FROSTSTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_GLACIALSTAFF.png?count=1&amp;quality=1"
                    draggable="true">
            </div>
        </div>

        <!-- Fogo -->
        <div class="main-items" style="position: relative;" data-category="fogos">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_FIRESTAFF_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_FIRE_RINGPAIR_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_FIRESTAFF_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_FIRESTAFF_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_FIRESTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_FIRESTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_INFERNOSTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_INFERNOSTAFF_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
            </div>
        </div>

        <!-- Bruxo -->
        <div class="main-items" style="position: relative;" data-category="bruxos">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_CURSEDSTAFF_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_CURSEDSTAFF_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_CURSEDSTAFF_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_CURSEDSTAFF_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_CURSEDSTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_CURSEDSTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_SKULLORB_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DEMONICSTAFF.png?count=1&amp;quality=1"
                    draggable="true">
            </div>
        </div>

        <!-- Arcos -->
        <div class="main-items" style="position: relative;" data-category="arcos">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_2H_BOW_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_BOW_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_BOW_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_LONGBOW.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_BOW_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_BOW.png?count=1&amp;quality=1" draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_WARBOW.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_LONGBOW_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
            </div>
        </div>


        <!-- Lancas -->
        <div class="main-items" style="position: relative;" data-category="lancas">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_SPEAR_LANCE_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_SPEAR_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_SPEAR.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_SPEAR.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_TRIDENT_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_GLAIVE.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_HARPOON_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_GLAIVE_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
            </div>
        </div>

        <!-- Bestas -->
        <div class="main-items" style="position: relative;" data-category="bestas">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_2H_CROSSBOW_CANNON_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_CROSSBOWLARGE_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_CROSSBOWLARGE.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_CROSSBOW.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DUALCROSSBOW_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_REPEATINGCROSSBOW_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DUALCROSSBOW_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MAIN_1HCROSSBOW.png?count=1&amp;quality=1"
                    draggable="true">
            </div>
        </div>

        <!-- Bordoes -->
        <div class="main-items" style="position: relative;" data-category="bordoes">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_2H_QUARTERSTAFF_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_QUARTERSTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_TWINSCYTHE_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_ROCKSTAFF_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_IRONCLADEDSTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DOUBLEBLADEDSTAFF_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_DOUBLEBLADEDSTAFF.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_2H_COMBATSTAFF_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
            </div>
        </div>

        <!-- Escudos -->
        <div class="main-items" style="position: relative;" data-category="escudos">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_SHIELD_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_SHIELD_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_SHIELD_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_SHIELD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_SPIKEDSHIELD_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_TOWERSHIELD_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">

            </div>
        </div>


        <!-- Livros -->
        <div class="main-items" style="position: relative;" data-category="tomos">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_TOME_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_BOOK.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_ORB_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_DEMONSKULL_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_CENSER_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_TOTEM_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">

            </div>
        </div>

        <!-- Tochas -->
        <div class="main-items" style="position: relative;" data-category="tochas">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_TORCH_CRYSTAL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_JESTERCANE_HELL.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_LAMP_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_TALISMAN_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_HORN_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_OFF_TORCH.png?count=1&amp;quality=1"
                    draggable="true">

            </div>
        </div>

        <!-- Capas -->
        <div class="main-items" style="position: relative;" data-category="capas">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_CAPEITEM_DEMON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_CAPEITEM_MORGANA.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_CAPEITEM_KEEPER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_CAPEITEM_UNDEAD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_CAPEITEM_HERETIC.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_CAPEITEM_SMUGGLER.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_CAPEITEM_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_CAPEITEM_FW_BRECILIEN.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_CAPEITEM_FW_CAERLEON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_CAPEITEM_FW_THETFORD.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_CAPEITEM_FW_MARTLOCK.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_CAPEITEM_FW_LYMHURST.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_CAPEITEM_FW_FORTSTERLING.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_CAPEITEM_FW_BRIDGEWATCH.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_CAPE.png?count=1&amp;quality=1" draggable="true">
            </div>
        </div>


        <!-- Comidas -->
        <div class="main-items" style="position: relative;" data-category="comidas">

            <div class="images">
                <img src="https://render.albiononline.com/v1/item/T8_MEAL_SANDWICH_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MEAL_SANDWICH_FISH.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MEAL_SANDWICH.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MEAL_STEW_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MEAL_STEW_FISH.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T8_MEAL_STEW.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T7_MEAL_OMELETTE_FISH.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T7_MEAL_OMELETTE.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T7_MEAL_ROAST_FISH.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T7_MEAL_ROAST.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T7_MEAL_OMELETTE_AVALON.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T7_MEAL_PIE_FISH.png?count=1&amp;quality=1"
                    draggable="true">
                <img src="https://render.albiononline.com/v1/item/T7_MEAL_PIE.png?count=1&amp;quality=1"
                    draggable="true">

            </div>
        </div>

    </div>

    <!-- <div style="margin:15px 0;">
        
        <button class="btn-main" onclick="loadFromTextbox()">📥 Carregar build</button>
        <button class="btn-main" onclick="copyHash()">📋 Copiar hash</button>
    </div> -->
    <div class="scroll-box-item">
        <div class="container" id="container">
        </div>
    </div>

    <footer class="footer">
        Criado por <strong>Marcos Barbosa</strong>
    </footer>

    <script>

        document.getElementById("helpBtn").addEventListener("click", () => {
            Swal.fire({
                title: " Dicas rápidas",
                html: `
            <div style="text-align:left; font-size:14px">
                <b>Numpad +</b> → Aumenta o Tier<br>
                <b>Numpad −</b> → Diminui o Tier<br>
                <b>,</b> → Diminui o Encantamento<br>
                <b>.</b> → Aumenta o Encantamento<br>
                <b>B</b> → Coloca/Remove MasterPiece no Item<br><br>
                <b>MouseHover</b> em uma imagem para editar<br>
                Alterações são salvas automaticamente
            </div>
        `,
                icon: "info",
                confirmButtonText: "Entendi",
                confirmButtonColor: "#3085d6"
            });
        });

        let hoveredImg = null;

        document.addEventListener("mouseover", e => {
            const img = e.target.closest("img[data-type='equip']");
            if (img) {
                hoveredImg = img;
            }
        });

        document.addEventListener("mouseout", e => {
            if (e.target === hoveredImg) {
                hoveredImg = null;
            }
        });


        document.addEventListener("keydown", e => {
            if (!hoveredImg || e.repeat) return;

            let src = hoveredImg.src;

            const tierMatch = src.match(/T([4-8])_/);
            if (!tierMatch) return;

            let tier = parseInt(tierMatch[1], 10);

            const qualityMatch = src.match(/quality=(\d+)/);
            let quality = qualityMatch ? parseInt(qualityMatch[1], 10) : 1;

            const atMatch = src.match(/@(\d+)\.png/);
            let atLevel = atMatch ? parseInt(atMatch[1], 10) : 0;

            if (e.code === "NumpadAdd") tier++;
            if (e.code === "NumpadSubtract") tier--;

            if (e.code === "Comma") atLevel--;
            if (e.code === "Period") atLevel++;

            if (e.code === "KeyB") quality = (quality === 1) ? 5 : 1;

            tier = Math.min(8, Math.max(4, tier));
            atLevel = Math.max(0, Math.min(4, atLevel));

            let newSrc = src
                .replace(/@(\d+)?\.png/, ".png")
                .replace(/\.png/, atLevel > 0 ? `@${atLevel}.png` : ".png")
                .replace(/T[4-8]_/, `T${tier}_`)
                .replace(/quality=\d+/, `quality=${quality}`);

            hoveredImg.src = newSrc;
            saveToHash();
        });





        function filterBlocks(value) {
            const search = value.toLowerCase().trim();

            document.querySelectorAll(".main-items").forEach(block => {
                const category = block.dataset.category?.toLowerCase() || "";

                if (category.includes(search)) {
                    block.style.display = "";
                } else {
                    block.style.display = "none";
                }
            });
        }

        const container = document.getElementById('container');

        container.addEventListener('mousedown', e => {
            document.body.style.overflow = 'hidden';
        });

        container.addEventListener('mouseup', e => {
            document.body.style.overflow = 'auto';
        });

        function toggleMainItems(button) {
            const parent = button.parentElement;
            const imagesDiv = parent.querySelector(".images");
            if (imagesDiv.style.display === "none") {
                imagesDiv.style.display = "flex";
                button.style.backgroundColor = "green";
            } else {
                imagesDiv.style.display = "none";
                button.style.backgroundColor = "orange";
            }
        }

        let classes = [
            "❤️‍🩹 Healer",
            "⚔️ DPS",
            "🤝 Suporte",
            "🛡️ Tank"
        ]


        let colors = {
            green: "rgba(25, 110, 60, 0.75)",
            red: "rgba(120, 30, 25, 0.75)",
            yellow: "rgba(150, 120, 5, 0.75)",
            blue: "rgba(20, 60, 120, 0.75)"
        };


        let draggedMainImage = null;

        function enableMainItemsDrag() {
            document.querySelectorAll(".main-items img").forEach(img => {
                img.draggable = true;
                img.removeEventListener("dragstart", mainImageDragStart);
                img.removeEventListener("dragend", mainImageDragEnd);

                img.addEventListener("dragstart", mainImageDragStart);
                img.addEventListener("dragend", mainImageDragEnd);
            });
        }

        function mainImageDragStart(e) {
            draggedMainImage = e.target;
            e.target.classList.add("dragging");
        }

        function mainImageDragEnd(e) {
            e.target.classList.remove("dragging");
            draggedMainImage = null;
        }


        function enableMainImageDrop() {
            document.querySelectorAll(".image-block .images").forEach(container => {
                container.removeEventListener("dragover", mainImageDragOver);
                container.removeEventListener("drop", mainImageDrop);

                container.addEventListener("dragover", mainImageDragOver);
                container.addEventListener("drop", mainImageDrop);
            });
        }

        function mainImageDragOver(e) {
            e.preventDefault();
        }

        function mainImageDrop(e) {
            e.preventDefault();
            if (!draggedMainImage) return;

            const container = e.currentTarget;

            const img = draggedMainImage.cloneNode(true);
            img.title = "Clique para remover";
            img.dataset.type = "equip";

            img.onclick = () => { img.remove(); saveToHash(); };

            if (container.children.length === 0) {
                container.appendChild(img);
            } else {
                const afterElement = getDragAfterImage(container, e.clientX);
                if (!afterElement) container.appendChild(img);
                else container.insertBefore(img, afterElement);
            }

            refreshImageDrag();
            saveToHash();
        }


        function refreshAllDrag() {
            refreshBlockDrag();
            refreshImageDrag();
            enableMainItemsDrag();
            enableMainImageDrop();
        }


        let draggedBlock = null;
        let draggedImage = null;

        function saveToHash() {
            const blocks = [];
            document.querySelectorAll(".image-block").forEach(block => {
                const images = [...block.querySelectorAll("img")].map(img => ({
                    src: img.src,
                    type: img.dataset.type || null
                }));
                blocks.push({ color: parseInt(block.dataset.color || 0), images });
            });
            const hash = btoa(unescape(encodeURIComponent(JSON.stringify(blocks))));
            document.getElementById("hashInput").value = hash;
        }

        function addBlock() {
            const block = document.createElement("div");
            block.className = "image-block";
            block.dataset.color = 0;
            block.innerHTML = `
                <button class="remove-block" title="Excluir Build" onclick="this.parentElement.remove(); saveToHash()">✖</button>
                <button class="btn-color" onclick="toggleColor(this); saveToHash()">🛡️ Classe</button>
                <div class="images"></div>
            `;
            document.getElementById("container").appendChild(block);
            refreshAllDrag();
            saveToHash();
        }



        function toggleColor(btn, forcedIndex = null) {
            const block = btn.closest(".image-block");
            const order = Object.keys(colors);

            let index;

            if (forcedIndex !== null) {
                // usa o índice informado
                index = forcedIndex % order.length;
            } else {
                // comportamento atual (ciclo)
                index = parseInt(block.dataset.color || 0);
                index = (index + 1) % order.length;
            }

            btn.innerHTML = classes[index];
            block.style.background = colors[order[index]];
            block.style.borderColor = order[index];
            block.dataset.color = index;
        }


        function hideAll() { document.body.classList.add("hide-ui"); }
        function showAll() { document.body.classList.remove("hide-ui"); }
        function copyHash() { navigator.clipboard.writeText(document.getElementById("hashInput").value); $.notify("Hash!", { className: "success" }); }
        function copyBuildUrl() { const hash = document.getElementById("hashInput").value.trim(); if (!hash) return; navigator.clipboard.writeText(`${location.origin}${location.pathname}?build=${hash}`); $.notify("Build copiada!", "success"); }

        function loadFromTextbox() {
            const input = document.getElementById("hashInput").value.trim();
            if (!input) return;
            try { loadFromHash(input); refreshAllDrag(); } catch (e) { alert("Hash inválido"); console.error(e); }
        }

        function loadFromHash(hash) {
            const blocks = JSON.parse(decodeURIComponent(escape(atob(hash))));
            const container = document.getElementById("container");
            container.innerHTML = "";

            blocks.forEach(blockData => {
                const block = document.createElement("div");
                block.className = "image-block";
                block.dataset.color = blockData.color || 0;

                block.innerHTML = `
            <button class="remove-block" title="Excluir Build" onclick="this.parentElement.remove(); saveToHash()">✖</button>
            <button class="btn-color" onclick="toggleColor(this); saveToHash()">${classes[blockData.color]}</button>
            <div class="images"></div>
        `;

                const imagesDiv = block.querySelector(".images");

                blockData.images.forEach(data => {
                    const img = document.createElement("img");
                    img.src = data.src;
                    img.dataset.type = data.type;
                    img.title = "Clique para remover";
                    img.onclick = () => { img.remove(); saveToHash(); };
                    imagesDiv.appendChild(img);
                });


                const order = Object.keys(colors);
                const colorKey = order[blockData.color];
                block.style.background = colors[colorKey];

                container.appendChild(block);
            });

            refreshAllDrag();
            saveToHash();
        }

        function getDragAfterImage(container, x) {
            const draggableElements = [...container.querySelectorAll("img:not(.dragging)")];

            return draggableElements.reduce((closest, child) => {
                const box = child.getBoundingClientRect();
                const offset = x - box.left - box.width / 2;
                if (offset < 0 && offset > closest.offset) {
                    return { offset: offset, element: child };
                } else {
                    return closest;
                }
            }, { offset: Number.NEGATIVE_INFINITY }).element;
        }

        document.addEventListener("DOMContentLoaded", () => {
            refreshAllDrag();
            const urlParams = new URLSearchParams(window.location.search);
            const buildHash = urlParams.get("build");
            if (buildHash) {
                loadFromHash(buildHash);
            }
        });


        function enableBlockDragAndDrop() {
            const container = document.getElementById("container");
            const blocks = container.querySelectorAll(".image-block");
            blocks.forEach(block => {
                block.setAttribute("draggable", true);
                block.removeEventListener("dragstart", blockDragStart);
                block.removeEventListener("dragend", blockDragEnd);
                block.removeEventListener("dragover", blockDragOver);
                block.removeEventListener("dragleave", blockDragLeave);
                block.removeEventListener("drop", blockDrop);
                block.addEventListener("dragstart", blockDragStart);
                block.addEventListener("dragend", blockDragEnd);
                block.addEventListener("dragover", blockDragOver);
                block.addEventListener("dragleave", blockDragLeave);
                block.addEventListener("drop", blockDrop);
            });
        }

        function blockDragStart(e) { draggedBlock = e.currentTarget; e.currentTarget.classList.add("dragging"); }
        function blockDragEnd(e) { e.currentTarget.classList.remove("dragging"); draggedBlock = null; saveToHash(); }
        function blockDragOver(e) { e.preventDefault(); e.currentTarget.classList.add("drag-over"); }
        function blockDragLeave(e) { e.currentTarget.classList.remove("drag-over"); }
        function blockDrop(e) {
            e.preventDefault(); e.currentTarget.classList.remove("drag-over");
            if (!draggedBlock || draggedBlock === e.currentTarget) return;
            const container = document.getElementById("container");
            const blocks = [...container.querySelectorAll(".image-block")];
            const draggedIndex = blocks.indexOf(draggedBlock);
            const targetIndex = blocks.indexOf(e.currentTarget);
            if (draggedIndex < targetIndex) { container.insertBefore(draggedBlock, e.currentTarget.nextSibling); }
            else { container.insertBefore(draggedBlock, e.currentTarget); }
            saveToHash();
        }

        function enableImageDragAndDrop() {
            document.querySelectorAll(".image-block").forEach(block => {
                const container = block.querySelector(".images");
                const images = container.querySelectorAll("img");
                images.forEach(img => {
                    img.draggable = true;
                    img.removeEventListener("dragstart", dragStartHandler);
                    img.removeEventListener("dragend", dragEndHandler);
                    img.addEventListener("dragstart", dragStartHandler);
                    img.addEventListener("dragend", dragEndHandler);
                });
                container.removeEventListener("dragover", dragOverHandler);
                container.addEventListener("dragover", dragOverHandler);
                container.removeEventListener("drop", imageDropHandler);
                container.addEventListener("drop", imageDropHandler);
            });
        }

        function dragStartHandler(e) { draggedImage = e.target; draggedImage.classList.add("dragging"); }
        function dragEndHandler(e) { draggedImage.classList.remove("dragging"); draggedImage = null; saveToHash(); }
        function dragOverHandler(e) { e.preventDefault(); }
        function imageDropHandler(e) {
            e.preventDefault();
            const container = e.currentTarget;
            const afterElement = getDragAfterImage(container, e.clientX);
            if (!draggedImage) return;
            if (afterElement == null) { container.appendChild(draggedImage); }
            else { container.insertBefore(draggedImage, afterElement); }
            saveToHash();
        }

        function getDragAfterImage(container, x) {
            const draggableElements = [...container.querySelectorAll("img:not(.dragging)")];

            return draggableElements.reduce((closest, child) => {
                const box = child.getBoundingClientRect();
                const offset = x - box.left - box.width / 2;
                if (offset < 0 && offset > closest.offset) {
                    return { offset: offset, element: child };
                } else {
                    return closest;
                }
            }, { offset: Number.NEGATIVE_INFINITY }).element;
        }

        function refreshBlockDrag() { enableBlockDragAndDrop(); }
        function refreshImageDrag() { enableImageDragAndDrop(); }

        window.addEventListener("load", () => {
            const params = new URLSearchParams(window.location.search);
            const build = params.get("build");
            if (!build) return;
            try {
                const fixedHash = build.replace(/ /g, '+');
                document.getElementById("hashInput").value = fixedHash;
                loadFromHash(fixedHash);
                refreshAllDrag();
            } catch (e) {
                console.error("Hash inválido na URL", e);
            }
        });
    </script>
</body>

</html>