# UCM
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Marvel: línea de tiempo del multiverso</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Oswald:wght@500;700&display=swap" rel="stylesheet">
<style>
:root{--bg:#0b0b0d;--panel:#141418;--fg:#ececf0;--mut:#9a9aa8;--red:#d6202b;--col:132px;--lab:176px}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--fg);font:14px/1.4 system-ui,-apple-system,"Segoe UI",sans-serif}
header{padding:18px 20px 6px}
h1{margin:0;font:700 26px/1.1 Oswald,Impact,sans-serif;letter-spacing:.02em}
header p{margin:6px 0 0;color:var(--mut);max-width:70ch}
#filters{display:flex;flex-wrap:wrap;gap:6px;padding:10px 20px}
#filters button{border:1px solid var(--c);background:transparent;color:var(--fg);padding:4px 10px;border-radius:999px;font:inherit;font-size:12px;cursor:pointer}
#filters button[aria-pressed=true]{background:var(--c);color:#000}
#filters button:focus-visible,.chip:focus-visible{outline:2px solid #fff;outline-offset:2px}
#info{margin:0 20px 10px;padding:10px 14px;background:var(--panel);border-left:3px solid var(--red);min-height:44px;color:var(--mut)}
#info b{color:var(--fg)}
#wrap{overflow:auto;max-height:calc(100vh - 210px);min-height:420px;border-top:1px solid #26262e}
#grid{display:grid;width:max-content;min-width:100%}
.corner,.yr{position:sticky;top:0;z-index:4;background:var(--bg);height:74px;display:flex;align-items:center;justify-content:center}
.corner{left:0;z-index:6;justify-content:flex-start;padding-left:20px;color:var(--mut);font-size:12px}
.yr::before{content:"";position:absolute;left:0;right:0;top:50%;height:3px;background:#2a2a32}
.yr span{position:relative;width:54px;height:54px;border-radius:50%;background:#1b1b20;border:3px solid var(--red);box-shadow:0 0 0 3px #3a3a42;display:flex;align-items:center;justify-content:center;font:700 14px Oswald,sans-serif;text-align:center;line-height:1}
.yr.soon span{font-size:11px}
.lab{position:sticky;left:0;z-index:3;background:var(--bg);padding:12px 10px 12px 20px;border-left:5px solid var(--c);font:500 14px/1.25 Oswald,sans-serif;display:flex;flex-direction:column;justify-content:flex-start}
.lab small{font:12px/1.3 system-ui,sans-serif;color:var(--mut);margin-top:3px}
.cell{padding:10px 5px;border-bottom:1px solid #1c1c22;position:relative;display:flex;flex-direction:column;gap:5px;min-height:62px}
.cell::before{content:"";position:absolute;left:0;right:0;top:0;height:2px;background:var(--c);opacity:.35}
.chip{--k:var(--c);text-align:left;font:inherit;font-size:12px;line-height:1.25;color:var(--fg);background:color-mix(in srgb,var(--k) 20%,#0b0b0d);border:0;border-left:3px solid var(--k);padding:5px 7px;cursor:pointer;border-radius:0 4px 4px 0}
.chip.s{font-style:italic}
.chip.s::before{content:"serie · ";color:var(--mut);font-style:normal;font-size:11px}
.chip.b::after{content:" ⑂";color:#ffc233;font-weight:700}
.chip:hover,.chip.on{background:color-mix(in srgb,var(--k) 45%,#0b0b0d)}
.hide{display:none!important}
footer{padding:12px 20px 28px;color:var(--mut);font-size:12px;max-width:90ch}
</style>
</head>
<body>
<header>
<h1>Marvel: línea de tiempo del multiverso</h1>
<p>Cada fila es una Tierra o una rama. Las columnas siguen el orden cronológico dentro de la historia, no el de estreno. El símbolo ⑂ marca ramas y cruces entre universos.</p>
</header>
<div id="filters" role="group" aria-label="Mostrar u ocultar líneas"></div>
<div id="info" aria-live="polite">Toca una película o serie para ver detalles.</div>
<div id="wrap"><div id="grid"></div></div>
<footer>Los años posteriores a 2023 en el MCU son aproximados y se pueden editar en el arreglo <code>E</code> del código. Kind: F película, S serie, y la letra B marca una rama. Las series de ABC y Netflix quedan fuera por su estatus incierto.</footer>
<script>
const L=[
["mcu","MCU","Tierra-616","#a78bfa"],
["rama","Ramas del MCU","Loki, What If, Tierra-838","#e879f9"],
["xo","X-Men Fox","Línea original","#14b8a6"],
["xn","X-Men Fox","Nueva línea tras DOFP","#67e8f9"],
["xd","Deadpool y Logan","Ramas sueltas","#fb923c"],
["raimi","Spider-Man Raimi","Tierra-96283","#ef4444"],
["webb","Spider-Man Webb","Tierra-120703","#60a5fa"],
["ssu","Universo Sony","Venom y compañía","#a3e635"],
["verse","Spider-Verse","Animado, Tierra-1610","#f472b6"],
["ffox","4 Fantásticos Fox","2005, 2007 y 2015","#34d399"],
["f828","4 Fantásticos MCU","Tierra-828","#facc15"]
];
const E=[
["mcu",1942,"Capitán América: El primer vengador","F"],
["mcu",1946,"Agent Carter","S"],
["mcu",1995,"Capitana Marvel","F"],
["mcu",2010,"Iron Man","F"],
["mcu",2011,"Iron Man 2","F"],["mcu",2011,"El increíble Hulk","F"],["mcu",2011,"Thor","F"],
["mcu",2012,"Los Vengadores","F","Aquí empieza la rama: Loki escapa con el Teseracto en el viaje en el tiempo de Endgame."],
["mcu",2013,"Thor: El mundo oscuro","F"],["mcu",2013,"Iron Man 3","F"],
["mcu",2014,"Capitán América: El soldado de invierno","F"],["mcu",2014,"Guardianes de la Galaxia","F"],["mcu",2014,"Guardianes de la Galaxia 2","F"],
["mcu",2015,"Los Vengadores: Age of Ultron","F"],["mcu",2015,"Ant-Man","F"],
["mcu",2016,"Capitán América: Civil War","F"],["mcu",2016,"Viuda Negra","F"],["mcu",2016,"Black Panther","F"],["mcu",2016,"Spider-Man: Homecoming","F"],["mcu",2016,"Doctor Strange","F"],
["mcu",2017,"Thor: Ragnarok","F"],
["mcu",2018,"Ant-Man y la Avispa","F"],["mcu",2018,"Vengadores: Infinity War","F"],
["mcu",2023,"Vengadores: Endgame","FB","Viaje en el tiempo (Time Heist): cada salto al pasado abre líneas alternas."],
["mcu",2023,"WandaVision","S"],
["mcu",2024,"Falcon y el Soldado de Invierno","S"],["mcu",2024,"Spider-Man: Lejos de casa","F"],["mcu",2024,"Shang-Chi","F"],["mcu",2024,"Eternals","F"],["mcu",2024,"Hawkeye","S"],
["mcu",2024,"Spider-Man: No Way Home","FB","Se rompe el hechizo del multiverso. Llegan los Spider-Man de Raimi y Webb y regresan a sus Tierras. Peter es olvidado por todos."],
["mcu",2025,"Moon Knight","S"],["mcu",2025,"Ms. Marvel","S"],["mcu",2025,"She-Hulk","S"],
["mcu",2025,"Doctor Strange en el multiverso de la locura","FB","Visita la Tierra-838 (Illuminati) y otras Tierras."],
["mcu",2025,"Thor: Love and Thunder","F"],["mcu",2025,"Black Panther: Wakanda Forever","F"],["mcu",2025,"Ant-Man y la Avispa: Quantumania","F"],
["mcu",2025,"Werewolf by Night","S"],["mcu",2025,"Your Friendly Neighborhood Spider-Man","S"],["mcu",2025,"Secret Invasion","S"],
["mcu",2026,"Guardianes de la Galaxia 3","F"],["mcu",2026,"The Marvels","F"],["mcu",2026,"Echo","S"],["mcu",2026,"Agatha","S"],
["mcu",2026,"Capitán América: Brave New World","F"],["mcu",2026,"Thunderbolts*","F","La escena post-créditos acerca a los 4 Fantásticos de la Tierra-828."],
["mcu",2026,"Ironheart","S"],["mcu",2026,"Daredevil: Born Again","S"],
["mcu",2026,"Deadpool y Lobezno","FB","La TVA poda la Tierra-10005 (Fox). Wade y un Wolverine variante cruzan al MCU."],
["mcu",2026,"Spider-Man: Brand New Day","F","Estrenada en julio de 2026. Marvel dice que ocurre antes de Doomsday."],
["mcu",9000,"Avengers: Doomsday","FB","Estreno 18 dic 2026. Reúne personajes de varias Tierras. Los X-Men de Fox están confirmados. Lo demás son rumores."],
["mcu",9000,"Avengers: Secret Wars","F","Estreno 17 dic 2027."],
["rama",2011,"What If…? (T1 a T3)","S","Series animadas con Tierras alternas."],
["rama",2012,"Loki (T1 y T2)","SB","La TVA poda la línea de Loki. La línea sagrada se rompe y el multiverso se abre."],
["rama",2025,"Tierra-838: Illuminati","FB","Doctor Strange 2."],
["xo",1979,"X-Men Orígenes: Wolverine","F","Abarca desde 1845 hasta 1979."],
["xo",2000,"X-Men","F"],["xo",2003,"X2","F"],["xo",2006,"X-Men: La decisión final","F"],["xo",2013,"Lobezno inmortal","F"],
["xo",2023,"DOFP: el futuro oscuro","FB","Los Centinelas devastan el mundo. Este futuro se borra cuando Wolverine cambia 1973."],
["xn",1962,"X-Men: Primera generación","F"],
["xn",1973,"X-Men: Días del futuro pasado","FB","Wolverine cambia el pasado y nace la nueva línea."],
["xn",1983,"X-Men: Apocalipsis","F"],["xn",1992,"X-Men: Dark Phoenix","F"],["xn",2020,"Los nuevos mutantes","F"],["xn",2023,"Nuevo futuro (final de DOFP)","F"],
["xd",2016,"Deadpool","F"],["xd",2018,"Deadpool 2","F","Cable viaja en el tiempo y Wade cambia las cosas."],
["xd",2026,"Deadpool y Lobezno","FB","El cruce con el MCU. Tierra-10005."],
["xd",2029,"Logan","FB","Rama aparte. Su encaje exacto con la nueva línea es ambiguo."],
["raimi",2002,"Spider-Man","F"],["raimi",2004,"Spider-Man 2","F"],["raimi",2007,"Spider-Man 3","F"],
["raimi",2024,"No Way Home: cruce a la 616","FB"],
["webb",2012,"The Amazing Spider-Man","F"],["webb",2014,"The Amazing Spider-Man 2","F"],
["webb",2024,"No Way Home: cruce a la 616","FB"],
["ssu",1933,"Spider-Noir","S","Serie ambientada en los años 30."],
["ssu",2003,"Madame Web","F"],["ssu",2018,"Venom","F"],["ssu",2020,"Morbius","F"],["ssu",2021,"Venom: Habrá matanza","F"],
["ssu",2024,"Venom: El último baile","FB","Eddie cruza brevemente al MCU y regresa."],["ssu",2024,"Kraven el cazador","F"],
["verse",2018,"Spider-Man: Un nuevo universo","F"],["verse",2023,"Spider-Man: Cruzando el multiverso","F"],
["verse",9000,"Beyond the Spider-Verse","F","Pendiente."],
["ffox",2005,"Los 4 Fantásticos","F","Tierra-121698."],["ffox",2007,"Los 4 Fantásticos y Silver Surfer","F"],["ffox",2015,"Los 4 Fantásticos (reboot)","F","Otra línea distinta."],
["f828",1965,"Los 4 Fantásticos: Primeros pasos","F","Tierra-828."],
["f828",9000,"Llegan a la Tierra-616","FB","Doom y Franklin. Conectan con Doomsday."]
];
const years=[...new Set(E.map(e=>e[1]))].sort((a,b)=>a-b);
const g=document.getElementById("grid"),info=document.getElementById("info"),fl=document.getElementById("filters");
g.style.gridTemplateColumns=`var(--lab) repeat(${years.length},var(--col))`;
const mk=(t,c,x)=>{const d=document.createElement(t);if(c)d.className=c;if(x!==undefined)d.textContent=x;return d};
g.append(mk("div","corner","Universo"));
years.forEach(y=>{const d=mk("div","yr"+(y>=9000?" soon":""));d.append(mk("span","",y>=9000?"Pronto":y));g.append(d)});
let cur=null;
L.forEach(([id,n,s,c])=>{
const row=[];
const lab=mk("div","lab");lab.style.setProperty("--c",c);lab.append(mk("span","",n),mk("small","",s));row.push(lab);
years.forEach(y=>{
const cell=mk("div","cell");cell.style.setProperty("--c",c);
E.filter(e=>e[0]===id&&e[1]===y).forEach(e=>{
const b=mk("button","chip"+(e[3][0]==="S"?" s":"")+(e[3].includes("B")?" b":""),e[2]);
b.type="button";
b.onclick=()=>{cur&&cur.classList.remove("on");cur=b;b.classList.add("on");
info.innerHTML="";const t=mk("b","",e[2]+" · "+(y>=9000?"próximamente":y));info.append(t,document.createTextNode(e[4]?" — "+e[4]:""))};
cell.append(b)});
row.push(cell)});
row.forEach(el=>{el.dataset.lane=id;g.append(el)});
const f=mk("button","",n+" · "+s);f.style.setProperty("--c",c);f.type="button";f.setAttribute("aria-pressed","true");
f.onclick=()=>{const on=f.getAttribute("aria-pressed")!=="true";f.setAttribute("aria-pressed",on);
g.querySelectorAll(`[data-lane="${id}"]`).forEach(el=>el.classList.toggle("hide",!on))};
fl.append(f)});
</script>
</body>
</html>
