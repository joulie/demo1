# Principe de l'appli : webservice
Vous devez réaliser un Web Service en langage PHP qui permet de récupérer les informations d’un « adhérent » à partir de son « identifiant ». Les données (liste des adhérents) sont stockées dans un fichier CSV.

# choix  du framework et des composants
SF4.1 : parce que symfony et je n'avais pas utilisé la V4
install du debug-pack SF4


choix d'une API REST : plus lisible que SOAP


<?php
// src/Controller/CsvController.php
namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\JsonResponse;

class CsvController extends Controller
{
    public function index()
    {
        $adherents = $this->getAdherents();
        return $this->customJsonEnconding($adherents);
    }

    public function getAdherentById($id)
    {
        $adherents = $this->getAdherents();
        $i=0;
        foreach ($adherents as $adherent) {
            $i++;
            foreach ($adherent as $key => $values) {
                if ($values == $id) {
                    $returnKey = $i;
                }
            }
        }

        return $this->customJsonEnconding($adherents);
    }

    public function getAllAdherentWithCount() {
        $adherents = $this->getAdherents();
        return $this->customJsonEnconding($adherents);
    }

    public function getAllAdherentWithCountSorted() {
        $adherents = $this->getAdherents();
        $this->sortAdherents($adherents);
        return $this->customJsonEnconding($adherents, $yesGiveCount = true);
    }

    private function getAdherents() {
        //$utilisateurs = array(); // Tableau qui va contenir les éléments extraits du fichier CSV
        $row = 0; // Représente la ligne

        // Import du fichier CSV
        if (($handle = fopen(__DIR__ . "/../../public/donnees.csv", "r")) !== FALSE) { // Lecture du fichier - ligne trouvée sur https://gist.github.com/RomainGarcia/364524566b562f86e90f
            $data = fgetcsv($handle, 1000, ";");
            if ($data !== FALSE) {
                $num = count($data); // Nombre d'éléments sur la ligne traitée
                for ($c = 0; $c < $num; $c++) {
                    $entete[$c] = $data[$c];
                }
            }

            while (($data = fgetcsv($handle, 1000, ";")) !== FALSE) { // Eléments séparés par un point-virgule, à modifier si necessaire
                $row++;
                for ($c = 0; $c < $num; $c++) {
                    $adherents[$row] = array(
                        $entete[0] => $data[0],
                        $entete[1] => $data[1],
                        $entete[2] => $data[2],
                        $entete[3] => $data[3]
                    );
                }
            }
            fclose($handle);

        }

        return $adherents;
    }

    private function sortAdherents($adherents) {
        $prenom = array();
        foreach ($adherents as $key => $row)
        {
            $prenom[$key] = $row['prenom'];
        }
        array_multisort($prenom, SORT_ASC, $adherents);

        $nom = array();
        foreach ($adherents as $key => $row)
        {
            $nom[$key] = $row['nom'];
        }
        return array_multisort($nom, SORT_ASC, $adherents);
    }

    private function customJsonEnconding($returnArray, $withCount = null) {
        $response = new Response(json_encode(isset($withCount) ? array('size' => sizeof($adherents))+$adherents : $returnArray, JSON_UNESCAPED_UNICODE));
        $response->headers->set('Content-Type', 'application/json');
        return $response;
    }
}