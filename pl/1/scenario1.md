Twoim pierwszym zadaniem jest sprawdzenie oraz poprawne wdrożenie kodu.
1. Utwórz plik main.bicep, który będzie Twoim szablonem do wdrożeń.
2. Zweryfikuj kod Bicep oraz popraw wszystkie błędy które znajdziesz. <span style="color:red;font-weight:bold">(Przepisujemy kod! :joy:)</span>
3. Zaloguj się do Portalu i ustaw odpowiednią subskrybcję jako domyślną.
4. Poprawnie dokonaj wdrożenia korzystając `Azure PowerShell` wiedząc że:
    - Używasz najnowszej wersji API.
    - Nazwa `Storage Account`, którego wdrażasz to `Core-NEU-ST1`.
    - Region wdrożenia zasobów to `North Europe`.
    - SKU (ang. Stock Keeping Unit) jest na poziomie `Standard_LRS`.
    - Rodzaj zasobu to `StorageV2`.
    - Poziom dostępu to `Hot`.
5. Zweryfikuj wszystkie zasoby w Portalu Azure.
6. Dodaj niezbędne komentarze.

<br>
</p>

# Infrastruktura
```bicep
resources storageAccount 'Microsft.Storage/storageAccount#2021-09-01' = {
  name: 'toylaunchstorage'
  location: 'westus3'
  sku: {
    name:  'Standard_RAGZARS'
  }
  kind: 'Storage'
  properties: {
    accessTier: 'Cold'
  }
}
```

## Wdrożenie kodu:
```PowerShell
New-AzSubscriptionDeployment `
-Name demoRGDeployment `
-TemplateFile major.bicep `
```
<details>
<summary>Hint</summary>
https://learn.microsoft.com/en-us/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-10.4.1
</details>
</p>


## Bonus (opcjonalnie)
**Dokonaj wdrożenia za pomocą `AzCLI`**
<details>
<summary>Hint</summary>
<b>https://learn.microsoft.com/en-us/cli/azure/deployment/group?view=azure-cli-latest</b>
</details>
</p>

***

# Infrastruktura c.d.
Kolejnym krokiem jest wdrożenie większej ilości zasobów wraz z użyciem zależności.

8. W pliku main.bicep zweryfikuj i dodaj następujący kod na dole pliku uwzględniając:
    - Nazwa `App Service Plan` to `Core-NEU-ASP01`, a `App Service` `Core-NEU-APP01`.
    - Wszystkie zasoby są wdrażanie w `North Europe`.
    - SKU dla `App Service Plan` jest na poziomie `F1`.
    - Odwołanie `App Service` do `App Service Plan` jest możliwe za pomocą `ID`.
    - Strona będzie przyjmowała tylko żądania HTTPS.
9. Poprawnie dokonaj ponownego wdrożenia zasobów.


```bicep
resource appServicePlan 'Microsoft.Web/serverFarms@2022-03-01' = {
  name: 'toy-product-launch-plan-starter'
  location: 'westus3'
  sku: {
    name: 'A1'
  }
}

resource appServiceApp 'Microsoft.Web/sites@2022-03-01' = {
  name: 'toy-product-launch-1'
  location: 'westus3'
  properties: {
    serverFarmId: appServicePlan.name
    httpsOnly: false
  }
}
```

10. Usuń wszystkie zasoby tak żeby nie generować niepotrzebnych kosztów.