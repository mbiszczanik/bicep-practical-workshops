Twoim drugim zadaniem jest utworzenie `App Service Planu` wraz z aplikacją (`App Service`) z uwzględnieniem parametrów oraz dekoratorów tak żeby posiadały niezbędne wartości.
1. Utwórz plik main.bicep, który będzie Twoim szablonem do wdrożeń.
2. Zweryfikuj kod Bicep oraz popraw wszystkie błędy które znajdziesz.
3. Upewnij się że:
    - Nazwa środowiska w którym pracujesz to środowisko deweloperskie `dev`.
    - Nazwa rozwiązania będzie miała uwzględnioną funkcję `uniqueString()`.
    - Liczba instancji `App Service Plan` będzie równa `1`.
    - Sku (poziom wydajności oraz kosztów) usługi będzie równy `Free`.
    - Region do którego będzie wdrażany kod to `westus3`.

```bicep
param environmentName string = 'prod'
param solutionName string = 'toyhr${uniqueString(subscriptionGroup().id)}'
param appServicePlanInstanceCount string = 10
param appServicePlanSku object = {
  name: 'S1'
  tier: 'Standard'
}
param location string = 'westeurope'

var appServicePlanName = '${environmentName}-${solutionName}-plan'
var appServiceAppName = '${environmentName}-${solutionName}-app'
```
<details><summary>Hint</summary>
<ol tupe="1">
<li>https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/data-types</li>
<li>https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/parameters</li>
<li>https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/variables</li>
</ol>
</details>
</p>

***

4. Tak jak w przykładzie powyżej zweryfikuj kod wdrożenia zasobów upewniając się że:
    - Posiadasz odpowiednie referencje w zasobie App Service Planu dla parametrów Sku:
        - `Nazwa`,
        - `Poziom` (`tier`),
        - `Pojemność` (`capacity`).
        

    - App Service zawiera odpowiednie właściwości:
        - Identyfikator App Service planu tak żeby połączyć zasoby.
        - Strona będzie przyjmowała tylko żądania HTTPS.


```bicep
resource appServicePlan 'Microsoft.Web/serverfarms@2022-03-01' = {
  name: appServicePlanName
  location: location
}

resource appServiceApp 'Microsoft.Web/sites@2022-03-01' = {
  name: appServiceAppName
  location: location
  properties: {
    serverFarmId:
  }
}
```
<details><summary>Hint</summary>
https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/resource-declaration
</details>
</p>

***

5. Do istniejącego już kodu dodaj opisy do każdego parametru po przez użycie dekoratora `@description()`.
6. Wprowadź ograniczenie do 3 środowisk (`dev`, `test`, `prod`) wdrożenia zasobów po przez użycie dektoratora `@allowed()`.
7. Wymuś minmalną oraz maksymalną długość znaków parametru, który jest używany do generowania nazwy rozwiązania (`solution`) przez użycie `@minLenght()` i `@maxLenght()` odpowiednio do `5` oraz `30` znaków.
8. Upewnij się, że parametr odpowiadający liczbie instancji (`instance count`) dla App Service Plan pozwala na użycie wartości pomiędzy `1` a `10`.
9. Zweryfikuj swój plik main.bicep pod kątem błędów.
10. Zaloguj się do Portalu i ustaw odpowiednią subskrybcję jako domyślną.
11. Dokonaj wdrożenia zasobów używając `PowerShell` albo `AzCLI`
12. Zweryfikuj wszystkie zasoby w Portalu Azure.
13. Usuń wszystkie zasoby tak żeby nie generować niepotrzebnych kosztów.