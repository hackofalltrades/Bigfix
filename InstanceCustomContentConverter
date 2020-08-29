$pathtobes = Read-Host "Enter path to content"

$oldsvr = Read-Host "Enter old hostname"

$newsvr = Read-Host "Enter new hostname"

 

 

 

 

$fixlets = get-childitem $pathtobes -recurse | where {$_.extension -eq ".bes"}

 

    foreach ($fixlet in $fixlets) {

        $fixletpath= $fixlet.FullName

        $bescontent = (Get-Content -literalpath "$fixletpath")

            if (($bescontent -like "*http://$oldsvr.yourdomain.com:52311*") -and ($bescontent -notlike "*http://download.microsoft.com*")) {

 

               

                (select-string "prefetch" "$fixletpath") | Foreach-object {

                $line =  $_ -replace 'prefetch',' !,prefetch'

                $line = $line.split("^")[0]

                $line = $line -replace '<ActionScript MIMEType="application/x-Fixlet-Windows-Shell">',""

                $line = $line.split(",")[1]

                $line >> final.csv

                $sha256=$line -replace 'sha1:',"^"

                $sha256= $sha256.split("^")[1]

                $sha256 = $sha256 -replace 'size:',"^"

                $sha256 = $sha256.split("^")[0].trim()

               

                $line = $line  -replace "prefetch $sha256","extract $sha256 __Download\TransferData\$sha256"

                $line = $line -replace 'sha1:',"^"

                $line= $line.split("^")[0]


                $line >> final.csv

                $copy= "copy \$sha256 e:\temp\$sha256\ /e /i"

              

              $copy >> copy.txt

              

                

                  

                }

 

               

                $bescontent = (Get-Content -literalpath "$fixletpath") -replace "$oldsvr","$newsvr"

                    Set-Content -literalpath "$fixletpath" $bescontent

                }

       

 

       

}

 

foreach ($fixlet in $fixlets) {

$fixletpath= $fixlet.FullName

        $bescontent = (Get-Content -literalpath "$fixletpath")

            if ($bescontent -like "*member of*") {

                Write-Host "WARNING: Automatic Group contained in old environment file $fixlet. Make sure to modify this after import."

 

                }

       

        }

 
