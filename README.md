# Before Central Package Management with Transitive Pinning
```powershell
C:\work\CPMTPDemo [master ≡ +1 ~0 -0 !]> git co -q 01_Works
C:\work\CPMTPDemo [(01_Works) +1 ~0 -0 !]> dotnet build
Restore complete (0.6s)
  CPMTPBug net472 succeeded (0.3s) → bin\Debug\net472\CPMTPBug.dll

Build succeeded in 1.1s
C:\work\CPMTPDemo [(01_Works) +1 ~0 -0 !]>
```
# After Central Package Management with Transitive Pinning
```diff
C:\work\CPMTPDemo [(01_Works) +1 ~0 -0 !]> git co -q 02_DoesNotWork
C:\work\CPMTPDemo [(02_DoesNotWork) +1 ~0 -0 !]> git diff -U0 01_Works 02_DoesNotWork
diff --git a/CPMTPBug.csproj b/CPMTPBug.csproj
index d5c181d..8e18e79 100644
--- a/CPMTPBug.csproj
+++ b/CPMTPBug.csproj
@@ -12 +12 @@
-    <PackageReference Include="HtmlSanitizer" Version="8.1.870" />
+    <PackageReference Include="HtmlSanitizer" />
diff --git a/__Directory.Packages.props b/Directory.Packages.props
similarity index 100%
rename from __Directory.Packages.props
rename to Directory.Packages.props
C:\work\CPMTPDemo [(02_DoesNotWork) +1 ~0 -0 !]>
```
```xml
C:\work\CPMTPDemo [(02_DoesNotWork) +1 ~0 -0 !]> cat .\Directory.Packages.props
<Project>
  <PropertyGroup>
    <ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
    <CentralPackageTransitivePinningEnabled>true</CentralPackageTransitivePinningEnabled>
    <CentralPackageVersionOverrideEnabled>false</CentralPackageVersionOverrideEnabled>
    <CentralPackageFloatingVersionsEnabled>true</CentralPackageFloatingVersionsEnabled>
  </PropertyGroup>
  <ItemGroup>
    <PackageVersion Include="AngleSharp" Version="1.2.0" />
    <PackageVersion Include="HtmlSanitizer" Version="8.1.870" />
  </ItemGroup>
</Project>
C:\work\CPMTPDemo [(02_DoesNotWork) +1 ~0 -0 !]>
```
```powershell
C:\work\CPMTPDemo [(02_DoesNotWork) +1 ~0 -0 !]> dotnet build
Restore complete (0.6s)
  CPMTPBug net472 failed with 1 error(s) (0.2s)
    C:\work\CPMTPDemo\Class1.cs(6,19): error CS0246: The type or namespace name 'IMarkupFormatter' could not be found (are you missing a using directive or an assembly reference?)

Build failed with 1 error(s) in 1.1s
C:\work\CPMTPDemo [(02_DoesNotWork) +1 ~0 -0 !]>
```