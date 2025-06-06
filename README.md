<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>TailorConnect</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      margin: 0;
      padding: 0;
    }
    header {
      background: #4CAF50;
      color: white;
      padding: 1em;
      text-align: center;
    }

    .container {
      padding: 2em;
      max-width: 500px;
      margin: auto;
      background: white;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      display: none;
    }

    button {
      padding: 10px 20px;
      background: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
      margin-top: 1em;
    }

    input {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
    }

    #tailorInfo {
      margin-top: 20px;
      font-weight: bold;
    }
  </style>
</head>
<body>

<header>
  <h1>TailorConnect</h1>
  <p>Connecter les tailleurs et les clients</p>
</header>

<div id="welcome" class="container" style="display: block;">
  <h2>Bienvenue</h2>
  <p>Appuyez sur le bouton pour vous connecter</p>
  <button onclick="afficherFormulaire()">Se connecter</button>
</div>

<div id="formulaireConnexion" class="container">
  <h2>Connexion</h2>
  <input type="text" id="identifiant" placeholder="Email ou numéro de téléphone" required>
  <input type="password" id="motdepasse" placeholder="Mot de passe" required>
  <button onclick="soumettreConnexion()">Continuer</button>
</div>

<div id="localisationEtResultat" class="container">
  <h2>Recherche du tailleur le plus proche...</h2>
  <p id="tailorInfo">En attente de la localisation...</p>
</div>

<script>
  // Liste de tailleurs fictifs (avec coordonnées GPS simplifiées)
  const tailleurs = [
    { nom: "Maison Kalsoum", tel: "+221774331936", adresse: "Scat urbam , Dakar", lat: 14.73646, lon: -17.45787 },
    { nom: "CoutureExpress", tel: "+221775761628", adresse: "25 Avenue des Ciseaux, Lyon", lat: 45.7640, lon: 4.8357 },
    { nom: "diagne couture", tel: "+221771211212", adresse: "8 Boulevard du Fil, Marseille", lat: 43.2965, lon: 5.3698 }
  ];

  function afficherFormulaire() {
    document.getElementById('welcome').style.display = 'none';
    document.getElementById('formulaireConnexion').style.display = 'block';
  }

  function soumettreConnexion() {
    const id = document.getElementById('identifiant').value;
    const mdp = document.getElementById('motdepasse').value;

    if (!id || !mdp) {
      alert("Veuillez remplir tous les champs.");
      return;
    }

    // Sauvegarde locale (simule une base de données)
    localStorage.setItem('utilisateur', JSON.stringify({ identifiant: id, motdepasse: mdp }));

    document.getElementById('formulaireConnexion').style.display = 'none';
    document.getElementById('localisationEtResultat').style.display = 'block';

    demanderLocalisation();
  }

  function demanderLocalisation() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(traiterPosition, erreurLocalisation);
    } else {
      document.getElementById('tailorInfo').textContent = "La géolocalisation n'est pas supportée.";
    }
  }

  function traiterPosition(position) {
    const userLat = position.coords.latitude;
    const userLon = position.coords.longitude;

    // Trouver le tailleur le plus proche (distance Euclidienne simplifiée)
    let plusProche = null;
    let distanceMin = Infinity;

    tailleurs.forEach(tailleur => {
      const d = Math.sqrt(Math.pow(userLat - tailleur.lat, 2) + Math.pow(userLon - tailleur.lon, 2));
      if (d < distanceMin) {
        distanceMin = d;
        plusProche = tailleur;
      }
    });

    document.getElementById('tailorInfo').innerHTML =
      `Tailleur le plus proche : <br>
       <strong>${plusProche.nom}</strong><br>
       Téléphone : ${plusProche.tel}<br>
       Adresse : ${plusProche.adresse}`;
  }

  function erreurLocalisation() {
    document.getElementById('tailorInfo').textContent = "Impossible d'obtenir votre localisation.";
  }
</script>

</body>
</html>


