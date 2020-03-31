### O problema

O plugin file injection é um um  recurso bastante utilizado no GLPI, porém está apresentando alguns  inconvenientes na versões da família 9.1.7.xx e  9.2 ,  começando no  processo de ativação já que a função de checagem  não está  homologada  para  as atuais versões.

A figura abaixo representa esta situação.

![Captura de tela de 2018-01-23 14-09-05](https://lh3.googleusercontent.com/p78IEeKg9ldk9UB6Wb3Cg3B-6Hv3yKfER4QCyIcIizg_ozAK4XMT2jt58NIoVL7V55QNUEpBNW983TJ7_pKDucSDO6T0Uh-qyRxocYNtQ3O4JPlDGuWbAgj2MmKDiYltBTTBMhTcskOmVaG3knjaBrZ33TZRDplA2nmbheiLYFw6TBfG3CzZtKAlbkAORhT0hCHL9Hjzp1pJA4-6j4_AsdKyGFZB7pr1QsTTv5UXo8B-AVyVICnluw3sXqZWnn34mBb-qSSIsuhqjgvIHOz2GQGS5fEq3_-eFB7rth4reI-XcP9l7bRaWhm1EaU5HZVBKRFp4BIr2DLTuW-j4mVn1k60UChrSQ22crcuK8HMlFgIQw9qJ53YMRkpjHadadaDNP62ueaagzTT59T0DAHRzRRb4Y5pHZHv6iEy9mSxURpNCAZcu5R_QziJShizKHVJbEKjo0Ar73AiRXSJB0MMnLarOdxHzc35mkeYrltj0i3GI6SpdMLWoDWSKQj3hJ6vgxmeOrEEaFwhU15e5PzoOULZMaYGuCzdcLB-ucqEUIa0c5cD8pHK-dBOoYnPISMXbrXUuAXpWyoKwzDv-Ma3NjZzdLCjWgK7ngKz0WUzyBjMJ2_Z7fQH-fBTejwd6fmrdA1spGVn59dwociRcT_gdoXrB0RxHDrP=w1400-h436-no)

### A solução

Se você deseja testar  este  plugin nas novas versões, basta alterar o   limite de pré requisitos na função de checagem, para isto basta inserir o seguinte comando;

```
sed 's/9.2/9.3/g' /var/www/html/glpi/plugins/datainjection/setup.php  |  tee  /var/www/html/glpi/plugins/datainjection/setup.php
```

Leve em consideração o local onde o glpi está  instalado  nesse caso em /var/www/html/glpi/

**Nota**  você também pode fazer essa alteração manualmente acessando o arquivo em

```
nano +88 /var/www/html/glpi/plugins/datainjection/setup.php
```

O comando acima  abre o arquivo posicionando o cursor na linha 88 exatamente onde devemos realizar a alteração sendo assim;

Altere  a  valor  **9.2** para **9.3**

Feito isso basta acessar  novamente o gestor de plugins do glpi e habilitá-lo

![habilitando-file-injecton](https://lh3.googleusercontent.com/uiZ6m2gON3ludSMGnkI1fTIL6wWY48SJccxCxlMn40eg-l_fWZDuWfOpZU5NIOGICx18onp5MEoS4dyZz5_z-moDA6MYg33DUl558m_GF3UImkuGcS3UqzmuuhUsk5-EspbyJQqSiqf__oZleW6vW_EQIVzr2oP5KpU_MV8r14wWiLKg2WKVYIRIh4NWlGsvoSFNgHQY6l1hT2M_kh15v2jTjiHdsvriMgGkGHoyTuRpnMqAghZjkIwka4WR1M7t6fG9692XQ1UkVqVvl4g-Uem-JvuDXEZTz653fPMhkbrKLIsd01tCi_NzRpG8UVkjkshPP0QV3Fjlijw74W18_E7RVwrnSy45Gt_fBGymolPgh4V2-RkGbFwWdAdYqKihmbB7iSMY0uPybbuW8fC21UDEZDzr3L5Bp7R4Lgj8FKaeTF-JxDJHVG-C3X9856jBuPakPNI6tUBFtHlPj2CBzkhv37hkTD_SCflcN-TFI1NEPBMpLf5mcLbh64TX6MfIVP6FtpqegFFViS0jAFevaKn1tvt_Q6A0Ko_Em2649Frt0y9PTI2VY5_QFr5xoBhFXMc4ec78t0Spp4NDmvhz2PJEV57oGt7cTkIBiKaYCnuKnRJ_OgrnT064QpcShPrWWL8wt6nM7G4Fo-XkAOQ33yl7nwenEiKb=w765-h342-no)

### Considerações Finais

Na maioria da vezes  há um motivo porque determinado plugin não funciona  em versões mais atuais, talvez controle de versão, correção de bugs,  incompatibilidade técnica,  na verdade N fatores, e neste  howto apenas  demonstramos como driblar  momentaneamente  esse obstáculo.

> Se esta solução serviu para você dê seu feedback comentando abaixo.

# ATUALIZAÇÕES IMPORTANTES

[Johan Cwiklinski](https://github.com/trasher) , membro do time de desenvolvedores de plugins do GLPI,  nos informou via [G Plus](https://plus.google.com/104503517546598303517/posts/Cwbm4sL3rUj?hl=pt-BR) o branch  para  Feature/compat 9.2   aberta abaixo

https://github.com/pluginsGLPI/datainjection/pull/48/commits

Foi pontuado também  que o trabalho ainda não está finalizado, problemas  como importação de SO’s podem acontecer e os restante apresenta um bom  funcionamento.

O importante é que agora poderemos acompanhar bem  de perto o status do datainjection um plugin muito importante para o  ecossistema do GLPI, ficaremos no aguardo, e obrigado Johan Cwiklinski  pela presteza no atendimento.
Fico por aqui e até uma próxima oportunidade. 😃