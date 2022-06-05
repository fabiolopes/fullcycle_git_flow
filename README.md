# Git flow

Comando *git flow init* define o init do git flow dentro do diretório.Devemos confirmar nomenclatura das branchs developer, feature, bugfix, release, hotfix, support, tag.

### Iniciando uma branch

Vamos iniciar a feature welcome:

`git flow feature start welcome`

Será criada a branch *feature/welcome* baseada em *develop*.

Imagina que criamos um arquivo index.html e salvamos, dando o commit nessa feature.


### Finalizando uma branch

Como já fizemos a alteração desejada na feature, finalizamos ela da seguinte forma:

`git flow feature finish welcome`

Ações:
- feature/welcome fez merge com develop;
- feature/welcome foi deletada localmente;
- git branch para develop;


### Criando uma release

`git flow release start 0.1.0`

Ações:
- release/0.1.0 criada a partir de develop;
- git branch para release/0.1.0;

Podemos notar que essa release contém asalterações mergeadas pela feature welcome.

Vamos criar outra feature agora.

`git flow feature start contact`

Criamos nele o arquivo contact.html. Logo após, finalizaremos

`git flow feature finish contact`

Ações:
- feature/contact fez merge com develop;
- feature/contact foi deletada localmente;
- git branch para develop;


Agora faremos checkout da release 0.1.0, alterando o arquivo index.html, e commitando. Então finalizamos a release:

`git flow release finish 0.1.0`

Ações:
Branch **release/0.1.0** fez merge com a **main**;
- Branch **release/0.1.0** deletada;
- Criação da tag **0.1.0**;
- release tag **0.1.0** fez merge com **develop**;

### Criação de hotfix

Criaremos essa branch de trabalho para correções em prod.
`git flow hotfix start welcome`

Ações:
- Criação de branch **hotfix/welcome** baseada na **main**;

Após alterar e commitar o arquivo necessário, vamos finalizar.

`git flow hotfix finish welcome`

Ações:
- **hotfix/welcome** fez merge com a **main**;
- Criação da tag de hotfix **welcome**;
- Tag de hotfix **welcome** fez merge em **develop**;


## Criando chave chave para assinatura digital de commits

Usaremos uma ferramenta chamada GPG (GNU Privacy Guard), para gerar nossa chave criptografana no WSL2.

`gpg --full-generate-key`

Opções:
- Tipo de chave (RSA and RSA);
- Tamanho da chave (4096);
- Tempo de expiração da chave (1 year);
- Identificação (usuário do git: fabiolopes);
- Email;

Output:
```
fabio@DESKTOP-EOD4T44:~/curso_fullcycle_git_flow$ gpg --full-generate-key
gpg (GnuPG) 2.2.19; Copyright (C) 2019 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
  (14) Existing key from card
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 1y
Key expires at Mon Jun  5 00:00:49 2023 -03
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: fabiolopes
Email address: fabiolopesbione@hotmail.com
Comment:
You selected this USER-ID:
    "fabiolopes <fabiolopesbione@hotmail.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: key F688FB0836D836F9 marked as ultimately trusted
gpg: directory '/home/fabio/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/home/fabio/.gnupg/openpgp-revocs.d/EF7BCF17EFF10C93AC76E510F688FB0836D836F9.rev'
public and secret key created and signed.

pub   rsa4096 2022-06-05 [SC] [expires: 2023-06-05]
      EF7BCF17EFF10C93AC76E510F688FB0836D836F9
uid                      fabiolopes <fabiolopesbione@hotmail.com>
sub   rsa4096 2022-06-05 [E] [expires: 2023-06-05]
```

Vamos listar a chave agora com o comando abaixo:

```
fabio@DESKTOP-EOD4T44:~/curso_fullcycle_git_flow$ gpg --list-secret-key --keyid-form LONG
gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
gpg: next trustdb check due at 2023-06-05
/home/fabio/.gnupg/pubring.kbx
------------------------------
sec   rsa4096/F688FB0836D836F9 2022-06-05 [SC] [expires: 2023-06-05]
      EF7BCF17EFF10C93AC76E510F688FB0836D836F9
uid                 [ultimate] fabiolopes <fabiolopesbione@hotmail.com>
ssb   rsa4096/74A4AE5AD9AA8154 2022-06-05 [E] [expires: 2023-06-05]
```

Sabemos que o id da chave é F688FB0836D836F9. Então podemos exportar.

```
fabio@DESKTOP-EOD4T44:~/curso_fullcycle_git_flow$ gpg --armor --export F688FB0836D836F9
-----BEGIN PGP PUBLIC KEY BLOCK-----

mQaNBGacHS4BEADdzFXttbTRnmFai1nHLjTRPcy64l9T32xR4wmn7QtzyVTFeJme
OUviS7vaDY7dTsbpBIoOhQGYEM4J0aB/Qxg6lMesbQLGZfESrVeXIJYxIkHMYL1a
...
-----END PGP PUBLIC KEY BLOCK-----
```

Para que essa chave seja configurada no github, precisamos copiar esse output da chave. Na noss página do github, acessar settings->SSH and GPG Keys->New GPG key. Vamos colar o texto da chave gerado pelo comando export e confirmar. Será solicitado a confirmação de login. Após isso, o github terá adicionada a chave.

Após isso, precisamos configurar no global user a chave criada via id da chave:

`git config --global user.signinkey ${id_da_chave}`

Precisamos adicionar o seguinte comando no arquivo ~/.bash_profile:

`export GPG_TTY=$(tty)`

Alguns comandos imprtantes para executar após:

```
fabio@DESKTOP-EOD4T44:~/curso_fullcycle_git_flow$ git config --global commit.pgpsign true
fabio@DESKTOP-EOD4T44:~/curso_fullcycle_git_flow$ git config --global tag.pgpSign true
```