#İşe yarayan temel Git notları ve alias kullanımı

#Git Notları
```
-- Developu (veya main repo) Güncelleme
git checkout develop
git fetch //remote değişiklikleri localine indirir ama uygulamaz
git pull //remote değişiklikleri çeker ve localine uygular


-- Branch Oluşturma
git branch feature/TASKID (branch ismi)

-- Branch Silme (Localden)
git branch -D feature/TASKID (branch ismi)

-- Olan Branch'e Geçme
git checkout feature/TASKID (branch ismi)

-- Olmayan Branch'i Oluşturup İçerisine Geçme
git checkout -b feature/TASKID (branch ismi)

-- Değişiklikleri Göndermek İçin Hazırlama
git add . //. tüm değişiklikleri ekler tek bir dosyayı göndermek istiyorsa . yerine dosyanın adını yazman gerek

-- Commit Ekleme
git commit -m "refs #TASKID ...."

-- Commit Mesajını Değiştirme
git commit --amend -m "New commit message"

-- Değişiklikleri Gönderme
git push -u origin feature/TASKID //githubda feature/TASKID yazılı sarı bir pencere göreceksin sağ tarafındaki yeşil butona tıklayarak pull request oluşturabilirsin.

-- Tek Commit Olarak PR Üzerinde Değişiklik Yapıp Gönderme
** PR açtın sonra yazdığın kodda değişiklik yapman gerekirse bu kısmı kullanabilirsin
** Bu şekilde yapmayadabilirsin ama yapmazsan aynı pr içerisinde birden fazla commit oluşur
git checkout feature/TASKID
//tüm değişikliklerini yap
git add .
git commit --amend --no-edit
git push -f origin feature/TASKID

-- Local Develop'u Remote Develop'a Eşitleme/Resetleme
git checkout develop
git reset --hard origin/develop

** Yukarıdaki kodla herhangi bir local branchi tekrar remote branche aşağıdaki kodlarla eşitleyebiliriz
git checkout feature/TASKID
git reset --hard origin/feature/TASKID

-- Birden Fazla Commitleri Birleştirme / Kaldırma
git checkout feature/TASKID
git rebase --interactive HEAD~2 //en son kaç commit üzerinde çalışalacaksa o kadar yaz headın yanına
//sonrasında commitlerin olduğu bir pencere açılacak. burada gerekli bilgilendirmeler var
//squashin s si birleştirmek
//pick ayrı olarak commitleri tutmak
//commit penceresinde istenmeye commitin başına # koy bu sayede o committeki değişiklikler alınmayacaktır.
git push -f origin feature/TASKID


-- Committeki Yapılan Değişiklikleri Görme
** Değişiklik yapıp github'a gönderdiysen ve tekrar çalışman gerekiyorsa değişiklik yaptığın tüm dosyaları tekrar görebilirsin.
git checkout feature/TASKID
git reset --soft HEAD~1 //1 commit olduğu için 1 yazdık, task içerisinde kaç commit varsa kaçının değişikliklerini görmek istiyorsan ona göre düzenlemen gerek
git commit -m "refs #taskid ...." //gönderirken tekrar commitle
git push -f origin feature/TASKID

-- Rebase / Conflict çözme / Branchi güncelleme vb.
** O anki branchi güncel developtaki değişikliklerle birlikte birleştirmek için kullanabilirsin. Conflict sorununu bu şekilde çözüyoruz.
git checkout develop
git pull //developu güncelleyelim
git checkout feature/TASKID
git rebase origin/develop
** Conflict varsa sol taraftaki git tabından sorunlu dosyalar görünecektir oralara tek tek bakıp sorunları çöz.
git add .
git rebase --continue //bundan sonra bir dosya açılacak o dosyadan commit ismini vb değiştirebilirsin pek gerek yok değişiklik yapmadan dosyayı kapat
git commit --amend --no-edit
git push -f origin feature/TASKID

-- Diff patch
git diff origin/develop > diff.patch //yapılan değişiklikleri diff.patch dosyasına atıyor.
git apply diff.patch //diffi uygulama

```
#Alias

Alttaki komutları metin belgesine kaydedip, uzantısını .ps1 (ms powershell dosya türü) olarak kaydet.
Ardından sağ tıklayıp "Run with Powershell" de.
Set-Alias -Name yazısından sonraki komut ilgili fonksiyonu çalıştırır.
Örneğin cmd'ye gs yazılırsa git status çalışır.
$args yazanlar arguman alır yani gcob branchIsmi yazılırsa git checkout -b branchIsmi kodu çalışır (branchIsmi isimli yeni branch oluşturur ve içerisine geçer)

```
Clear-Host

function cdSrcMaster {
    cd 'src/master'
}
Set-Alias -Name cds -Value cdSrcMaster -Force -Option AllScope

function gitStatus {
    git 'status'
}
Set-Alias -Name gs -Value gitStatus -Force -Option AllScope

function gitCheckout {
    git 'checkout' $args
}
Set-Alias -Name gco -Value gitCheckout -Force -Option AllScope

function gitCheckoutBranch {
    git 'checkout' '-b' $args
}
Set-Alias -Name gcob -Value gitCheckoutBranch -Force -Option AllScope

function gitLogOneLine {
    git 'log' '--oneline'
}
Set-Alias -Name gll -Value gitLogOneLine -Force -Option AllScope

function gitLog {
    git 'log' $args
}
Set-Alias -Name gl -Value gitLog -Force -Option AllScope

function gitAddAll {
    git 'add' '.'
}
Set-Alias -Name ga -Value gitAddAll -Force -Option AllScope

function gitCommit {
    git 'commit' '-m' $args
}
Set-Alias -Name gcm -Value gitCommit -Force -Option AllScope

function gitCommitAmend {
    git 'commit' '--amend' '--no-edit' 
}
Set-Alias -Name gcan -Value gitCommitAmend -Force -Option AllScope

function gitPushOrigin {
    git 'push' 'origin' $args
}
Set-Alias -Name gpho -Value gitPushOrigin -Force -Option AllScope

function gitPull {
    git 'pull' $args
}
Set-Alias -Name gp -Value gitPull -Force -Option AllScope

function gitFetch {
    git 'fetch'
}
Set-Alias -Name gf -Value gitFetch -Force -Option AllScope

function gitClone {
    git 'clone' $args
}
Set-Alias -Name gcl -Value gitClone -Force -Option AllScope

function gitStash {
    git 'stash' '-u'
}
Set-Alias -Name gst -Value gitStash -Force -Option AllScope

function gitStashPop {
    git 'stash' 'pop'
}
Set-Alias -Name gsp -Value gitStashPop -Force -Option AllScope

function gitReset {
    git 'reset' '--hard' $args
}
Set-Alias -Name grs -Value gitReset -Force -Option AllScope

# --------- npm ---------
Function ns{ npm start }
```
