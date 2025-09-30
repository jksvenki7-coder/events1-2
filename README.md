# events1-2<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Event & Catering Packages</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f9faff; color: #1a2236; margin: 0; padding: 0; }
    .container { max-width: 770px; margin: 32px auto 20px; background: #fff; padding: 32px 24px 28px; border-radius: 14px; box-shadow: 0 2px 10px #0001; }
    h1 { text-align: center; margin-bottom: 23px; }
    .menu-bar { font-weight: bold; font-size: 1.05em; margin-bottom: 18px; padding-left: 7px; }
    .add-display { border: 2px dashed #b4c4d9; border-radius: 6px; color: #25477e; background: #f6f9fc; text-align: center; padding: 14px 0; margin-bottom: 20px; font-size: 1.03em; }
    .packages-row { display: flex; gap: 14px; justify-content: center; margin-bottom: 23px; }
    .pkg-btn { flex: 1; min-width: 105px; background: #eef2fc; border: none; border-radius: 10px; padding: 15px 10px; font-size: 1.08em; text-align: center; font-weight: bold; cursor: pointer; transition: background .16s, color .14s; box-shadow: 0 1px 6px #312f4a10; position: relative; }
    .pkg-btn.selected { background: #1763b0; color: #fff; }
    .pkg-range { font-size: 0.85em; color: #565b66; margin-top: 6px; display:block; font-weight:normal; }
    .service-section { padding-top: 17px; }
    .service-list { display: flex; flex-wrap: wrap; gap: 12px; justify-content: flex-start; margin-bottom: 8px;}
    .service-item { padding: 12px 15px; background:#f3f6fe; border-radius:7px; box-shadow:0 2px 9px #4264c210; display: flex; align-items: center; font-size: 1em; min-width:140px; justify-content: space-between;}
    .service-toggle { margin-left: 12px; padding: 6px 12px; border: none; border-radius: 5px; background: #eee; color: #164776; font-weight: 600; cursor: pointer; transition: background .16s, color .16s;}
    .service-toggle.active { background: #219c51; color: #fff; }
    .service-toggle.inactive { background: #da2733; color: #fff; }
    .chosen-services { margin-top: 16px; background: #f2f7fc; border-radius:8px; padding: 13px 17px; box-shadow: 0 1px 7px #2158b10a; }
    .chosen-services ul { padding-left: 18px; }
    @media (max-width: 700px) {
      .container { padding: 10px 3vw 16px;}
      .packages-row { flex-direction: column;}
      .pkg-btn { min-width: 100px; margin-bottom: 8px;}
      .service-item { min-width: 90px;}
      h1 { font-size: 1.1em; }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Event & Catering Packages</h1>
    <div class="menu-bar">Menu</div>
    <div class="add-display">Add display</div>
    <div class="packages-row" id="pkgRow"></div>
    <div class="service-section" id="serviceSection" style="display:none;">
      <div style="font-weight:bold; margin-bottom:7px;" id="pkgTitle"></div>
      <div class="service-list" id="serviceList"></div>
      <div class="chosen-services" id="chosenServices" style="display:none;">
        <strong>Selected Services:</strong>
        <ul id="chosenList"></ul>
      </div>
    </div>
  </div>
  <script>
    const pkgs = [
      { id: "silver", name: "Silver", range: "50,000 - 1,00,000" },
      { id: "gold", name: "Gold", range: "1,00,000 - 3,00,000" },
      { id: "diamond", name: "Diamond", range: "3,00,000 - 5,00,000" },
      { id: "platinum", name: "Platinum", range: "5,00,000 - 8,00,000" }
    ];
    const services = [
      "Decoration", "Catering", "Photography", "Venue", "Music", "Lighting",
      "Anchor/Artist", "DJ", "Return Gift"
      // Add more service names here as needed
    ];
    let selectedPkg = null;
    let selectedServices = {};

    // Render packages
    const pkgRow = document.getElementById("pkgRow");
    pkgs.forEach(pkg => {
      const btn = document.createElement("button");
      btn.className = "pkg-btn";
      btn.innerHTML = `${pkg.name}<span class="pkg-range">${pkg.range}</span>`;
      btn.onclick = () => selectPkg(pkg, btn);
      pkgRow.appendChild(btn);
    });

    function selectPkg(pkg, btn) {
      selectedPkg = pkg;
      selectedServices = {};
      Array.from(pkgRow.children).forEach(b => b.classList.remove("selected"));
      btn.classList.add("selected");
      document.getElementById("serviceSection").style.display = '';
      document.getElementById("pkgTitle").textContent = `Package: ${pkg.name}`;
      renderServices();
      renderChosen();
    }

    function renderServices() {
      const sDiv = document.getElementById("serviceList");
      sDiv.innerHTML = '';
      services.forEach(svc => {
        const wrap = document.createElement("div");
        wrap.className = "service-item";
        const label = document.createElement("span");
        label.textContent = svc;
        const b = document.createElement("button");
        b.className = "service-toggle";
        b.textContent = selectedServices[svc] ? "Remove (−)" : "Add (+)";
        b.classList.toggle("active", !!selectedServices[svc]);
        b.classList.toggle("inactive", !selectedServices[svc]);
        b.onclick = () => {
          if(selectedServices[svc]) {
            delete selectedServices[svc];
          } else {
            selectedServices[svc] = true;
          }
          renderServices();
          renderChosen();
        };
        wrap.appendChild(label);
        wrap.appendChild(b);
        sDiv.appendChild(wrap);
      });
    }

    function renderChosen() {
      const chosen = document.getElementById("chosenServices");
      const list = document.getElementById("chosenList");
      const keys = Object.keys(selectedServices);
      if(keys.length) {
        chosen.style.display = "";
        list.innerHTML = '';
        keys.forEach(svc => {
          const li = document.createElement("li");
          li.textContent = svc;
          list.appendChild(li);
        });
      } else {
        chosen.style.display = "none";
        list.innerHTML = '';
      }
    }
  </script>
</body>
</html>
