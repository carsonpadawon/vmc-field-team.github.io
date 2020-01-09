---
layout: single
title: "Hybrid Cloud Extension (HCX)"
categories: labs
date: 2019-02-21
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
---
# Introdução

Neste exercício de laboratório, você aprenderá sobre o Hybrid Cloud Extension (HCX). Essa é uma ferramenta, integrada ao VMware Cloud on AWS, que permitirá migrar cargas de trabalho em massa para o VMware Cloud na AWS e reduzir significativamente o tempo e a complexidade da movimentação cargas de trabalho para a nuvem pública.

## O que é o Hybrid Cloud Extension (HCX)

![HCX1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX1.jpg)

O HCX abstrai recursos locais e em nuvem e os apresenta aos aplicativos como uma nuvem híbrida contínua. Com isto, o Hybrid Cloud Extension oferece interconexões multisite de alto desempenho, seguras e otimizadas. A abstração e as interconexões criam uma infraestrutura híbrida. Com isto, o Hybrid Cloud Extension facilita a mobilidade de aplicativos e a recuperação de desastres, de forma segura e sem problemas, em plataformas vSphere locais e nuvens VMware. O Hybrid Cloud Extension é um serviço multi-site, multi-nuvem, facilitando a verdadeira nuvem híbrida.

### Funcionalidades do Hybrid Cloud Extension

![HCX2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX2.jpg)

#### Mobilidade de aplicativo para qualquer nuvem vSphere

* Mover rapidamente as cargas de trabalho existentes de uma plataforma vSphere para o SDDC no VMware Cloud on AWS
* Reduza o tempo de planejamento inicial para análise de custo e recursos
* Acelere a adoção da nuvem e evite a adaptação do Data Center. Continuidade dos negócios com menor TCO

#### Continuidade de negócios com menor TCO

* O remapeamento de endereços IP e MAC não é necessário
* Não há necessidade de modernizar o ambiente de VM existente
* A extensão de nuvem híbrida fornece migração a quente (vMotion) ou a frio (Cold Migration) e migração bidirecional
* Extensão de nuvem híbrida simplifica seu modelo operacional

#### Arquitetado para segurança

* Garantir a segurança de nuvens privadas e públicas
* Proteger recursos com através de recuperação de desastres de forma resiliente
* Hybrid Cloud Extension permite a portabilidade de práticas de segurança e rede corporativa para a nuvem
* As políticas de segurança migram com aplicativos de infraestrutura de alto desempenho

#### Infraestrutura Híbrida de Alto Desempenho

* O otimizador da WAN embutido é ajustada para as necessidades de casos de uso híbridos
* Extensão de nuvem híbrida fornece roteamento ágil e inteligente
* Vários modelos de migração de VMs (incluindo o vMotion, Bulk, Cold Migration) facilitam a migração para e da nuvem sem nenhuma alteração

## Configurando HCX

Clique no link abaixo para saber como instalar e configurar o HCX dentro do seu ambiente on-prem vCenter.

[HCX Install and Configuration](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX-OnPrem-Installation.htm){:target="_blank"}

## HCX - Migração vMotion

Agora que você está familiarizado com a instalação e configuração do HCX. Vamos fazer uma migração real do vMotion (ao vivo) de uma máquina virtual para o VMware Cloud na AWS.

### Logando no On-Prem vCenter

Fornecemos um vCenter local com máquinas virtuais para migrar. Com base no seu Student ##, selecione a VM apropriada para migrar.

Na área de trabalho do Horizon (desktop.vmc.ninja), abra o google e acesse o vCenter local

** Observação: Consulte a página de acesso do aluno para fazer login na sua área de trabalho do Horizon https://vmc-field-team.github.io/student-access/**

![HCX01](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx01.jpg)

1. Abra o Google e digite https://vcenter-workshop.workshop.set.local/ui para o URL.
2. Digite suas credenciais de estudante (student#@set.local).
3. Digite sua senha atribuída a você.
4. Clique em **Login** para continuar.

### Migrarndo a máquina virtual para a nuvem

![HCX02](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx02.jpg)

1. Clique na seta para expandir **Datacenter**.
2. Clique na seta para expandir o cluster **VSAN-Cluster**.
3. Clique na seta para expandir o pool de recursos **Migrate**.
4. Clique na máquina virtual **Student##**.
5. Anote o **Endereço IP** para efetuar o ping mais tarde.

![HCX02-1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx02-1.jpg)

1. Clique com o botão direito no ícone do Windows no canto inferior esquerdo da sua área de trabalho.
2. Clique em **Command Prompt**.

![HCX02-2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx02-2.jpg)

1. Caso você não tenha capturado o endereço IP da máquina virtual.
2. Digite o prompt de comando **ping -t 172.60.2.xx**.

![HCX03](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx03.jpg)

1. Volte para o console do vCenter e clique com o botão direito do mouse na sua máquina virtual **Student##**.
2. Passe o mouse sobre **Hybridity Actions**.
3. Clique em **Migrate to the Cloud**.

![HCX04](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx04.jpg)

Para migrar sua máquina virtual para o VMware Cloud na AWS, você terá que selecionar os a pasta de destino, o pool de recursos e o armazenamento de dados.

**Nota: Certifique-se de selecionar a pasta Migrate e o pool de recursos para garantir que você possa encontrar a mesma máquina virtual ao migrar de volta para a localidade.**

1. Clique em **Specify Destination Folder** e selectione a pasta **Migrate**.
2. Clique em **Specify Destination Container** e selectione o Resource Pool **Migrate**.
3. CLique em **Select Storage** e selecione o Datastore **WorkloadDatastore**.
4. Clique em **Select Migration Type** e selecione **Cloud Motion with vSphere Replication**.
   Não modifique o **schedule failover** para garantir que a migração aconteça imediatamente.
5. Clique **Next** para validar sua configuração.

![HCX05](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx05.jpg)

1. Verifique se a validação foi bem sucedida.
2. Clique em **Finish** para migrar sua máquina virtual para o VMware Cloud na AWS.

### Verificar o progresso da migração

![HCX06](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx06.jpg)

1. Clique em **Menu**.
2. Clique em **HCX**.

![HCX07](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx07.jpg)

O painel de controle fornece o número de máquinas virtuais migradas, em andamento e agendadas.

1. Clique em **Migration**.

![HCX08](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx08.jpg)

1. Veja o progresso da migração do vMotion.
2. Clique no botão **Refresh** para atualizar o progresso.

![HCX09](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx09.jpg)

Enquanto a migração está em andamento, vamos ver a resposta do ping.

1. Clique no **Command Prompt** para retornar ao teste de ping.
2. Observe o teste de ping deixado em execução a partir de uma etapa anterior e observe que ele não caiu.

**Observação: Verifique se a migração foi bem-sucedida antes de prosseguir para a próxima etapa.**

Depois que a máquina virtual for migrada com êxito para o VMware Cloud na AWS, leve a mesma máquina virtual e migre-a de volta ao vCenter local.

### Migrar a máquina virtual de volta ao Data Center Local

Usaremos o vMotion para migrar a máquina virtual de volta ao vCenter local. Por favor, note que esta é uma operação serializada e dependendo de quantos estão realizando o vMotion de volta, pode levar algum tempo para ser concluído.

![HCX010](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx010.jpg)

1. Verifique se a máquina virtual foi migrada para o SDDC no VMware Cloud na AWS.
2. Clique no botão **Migrate Virtual Machines**.

![HCX011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx011.jpg)

1. Clique em **Reverse Migration** para alternar para o VMware Cloud no AWS vCenter.
2. Clique no Resource Pool **Migrate** para exibir as máquinas virtuais migradas.
3. Clique em **Student##**.
4. Clique em **Specify Destination Folder** e selecione a pasta **Migrate**.
5. Clique em **Specify Destination Container** e selecione o Resoure Pool **Migrate**.
6. Clique em **Select Storage** e selecione o Datastore **vsanDatastore**.
7. Clique em **Select Migration Type** e selecionet **vMotion**.
8. Clique **Next** para validar sua seleção.

![HCX012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx012.jpg)

1. Verifique se a validação foi bem sucedida.
2. Clique em **Finish** para migrar sua máquina virtual de volta para o seu vCenter local.

![HCX013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx013.jpg)

1. Verifique o progresso da migração do vMotion.
2. Clique no botão **Refresh** para atualizar o progresso.

![HCX014](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx014.jpg)

**Opcional**

1. Clique no **Command Prompt** para retornar ao teste de ping.
2. Observe o teste de ping deixado em execução a partir de uma etapa anterior e observe que ele não caiu durante a migração para o vCenter no local.
3. Use ctrl-c para cancelar o ping.

### Outros métodos de migração

Sinta-se à vontade para experimentar os outros tipos de migração. Use a mesma máquina virtual **Student##** e siga as mesmas etapas, mas em vez de vMotion, tente **Bulk Migration** ou **Cloud Motion**.

![HCX015](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx015.jpg)

#### Cloud Motion with vSphere Replication

Esta última opção oferece tempo de inatividade zero para a mobilidade da carga de trabalho da origem para o destino. Primeiro, os discos de carga de trabalho são replicados para o site de destino. A replicação é tratada usando a replicação integrada do vSphere do HCX. Esse processo depende da quantidade de dados e da largura de banda de rede disponível. Quando a sincronização de dados estiver concluída, o HCX inicia um vMotion. O vMotion migra a carga de trabalho para o site de destino e sincroniza apenas os dados restantes (delta) e o estado da memória de carga de trabalho. Existe uma opção para agendar uma janela de manutenção para a sincronização de dados do vMotion, caso contrário, isso acontece imediatamente.

#### Bulk Migration

A migração em massa (Bulk Migration) cria uma nova máquina virtual no site de destino. Isso pode ser local ou no VMC e mantém o UUID da carga de trabalho. Em seguida, ele usa o vSphere Replication para copiar a carga de trabalho do site de origem para o destino enquanto a carga de trabalho ainda está ligada. Nesse caso, um ponto do tempo do estado do disco de carga de trabalho, mas não do  tradicional do vSphere. A migração em massa é gerenciada pelo o gateway de nuvem de interconexão HCX. Durante a sincronização de dados, não há interrupção nas cargas de trabalho. A sincronização de dados depende da quantidade de dados e da largura de banda disponível. Existe uma opção para agendar uma janela de manutenção para a mudança, caso contrário, a mudança ocorre imediatamente. Depois que a sincronização de dados inicial é concluída, ocorre uma mudança (a menos que seja agendada). As cargas de trabalho do site de origem são desativadas e desligadas usando o VMware Tools. Se o VMware Tools não estiver disponível, o HCX solicitará que você desligue a(s) carga(s) de trabalho para iniciar a migração. Durante o processo de migração, uma sincronização delta ocorre com base no rastreamento de bloco alterado (CBT) para sincronizar as alterações desde o original. As cargas de trabalho no site de destino começarão a ligar assim que a sincronização de dados for concluída (incluindo alterações de dados delta). Há verificações em vigor para garantir que os recursos estejam disponíveis para ligar as cargas de trabalho. Se uma carga de trabalho de destino não puder ser ativada devido a recursos, a carga de trabalho de origem será ativada novamente.

#### vMotion "Live Migration"

O HCX suporta o vMotion que conhecemos e amamos . As cargas de trabalho são migradas ao vivo sem tempo de inatividade semelhante ao Cloud Motion com o vSphere Replication. O vMotion não deve ser usado para migrar centenas de cargas de trabalho ou cargas de trabalho com grandes quantidades de dados. Em vez disso, use o Cloud Motion com o vSphere Replication ou o Bulk Migration. Geralmente, uma rede do vMotion precisa ser configurada e roteada para o host do vSphere de destino, nesse caso, o tráfego do vMotion é manipulado pelo gateway de nuvem do HCX Interconnect para o vMotion entre nuvens. O vMotion através do HCX encapsula e criptografa todo o tráfego da origem ao destino, removendo a complexidade da rede de roteamento para a nuvem.

O HCX tem uma opção interna para manter o endereço MAC das cargas de trabalho. Se esta opção não estiver marcada, as cargas de trabalho terão um MAC diferente no site de destino. As cargas de trabalho devem estar em compatibilidade (hardware) versão 9 ou superior e 100 Mbps ou acima da largura de banda devem estar disponíveis. Com o vMotion e a migração bidirecional, é importante considerar o Enhanced vMotion Compatibility (EVC). A boa notícia é que a HCX também lida com a EVC. As cargas de trabalho podem ser migradas sem problemas e, uma vez reinicializadas, herdarão os recursos da CPU do cluster de destino. Isso permite um vMotion cross-cloud entre diferentes versões de chipsets (por exemplo, Sandy Bridge para Skylake), mas dentro da mesma família de processadores (por exemplo, Intel).
**Além disso, é importante observar que o vMotion é feito de maneira serializada.
**Apenas um vMotion ocorre por vez e enfileira as cargas de trabalho restantes até que o vMotion atual seja concluído.
