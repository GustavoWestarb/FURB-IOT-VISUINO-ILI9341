# Arduino + Visuino: Relógio no display

A seguir iremos ver a construção de um relógio usando o display [nome_do_display] usando o arduino virtual no Proteus mais o Visuino para montagem da lógica por trás.

## No Proteus

No Proteus, deveremos  iniciar um novo projeto através da aba file > open sample proect.

Na aba que abri devemos selecionar a opção **VSM for AVR**  na aba de categoria e posteriormente o resultado **36** na aba de resultados.

![Arduino%20Visuino%20Rel%20gio%20no%20display/Untitled.png](Arduino%20Visuino%20Rel%20gio%20no%20display/Untitled.png)

Clicamos em abrir e um projeto com os componentes necessários irá abrir no Proteus.

![Arduino%20Visuino%20Rel%20gio%20no%20display/Untitled%201.png](Arduino%20Visuino%20Rel%20gio%20no%20display/Untitled%201.png)

## No Visuino

Primeiramente devemos adicionar o componente **ILI9341** ao projeto

 

![Arduino%20Visuino%20Rel%20gio%20no%20display/Untitled%202.png](Arduino%20Visuino%20Rel%20gio%20no%20display/Untitled%202.png)

As ligações dele ficam da seguinte forma:

> Control = SPI In.
Chip Select = Digital 10.
Data Command = Digital 9.

### Componente de Hora.

Para colocar a hora na tela devemos adicionar os seguintes componentes ao Visuino: 

- **DS1307**.
- **DecodeDateTime**
- **FormattedText**

Ligamos o OUT do DS1307 ao In do DecodeDateTime e seu Control no no IN do I2C Channels.

Adicionamos 3 campos do tipo Unsigned Element ao FormattedText, uma para Hora, outro para Minuto e outro para o segundo e colocamos a seguinte formatação no componente: **%0:%1:%2**

Puxamos então as ligações **Hour**, **Minute** e **Second do** DecodeDateTime para os correspondentes Unsigned Elements no FormattedText.

![Arduino%20Visuino%20Rel%20gio%20no%20display/Untitled%203.png](Arduino%20Visuino%20Rel%20gio%20no%20display/Untitled%203.png)

 

Para finalizar o relógio adicionamos um elemento text no **ILI9341,** colocamo sua cor de fundo (fill color) como aclFuchsia e fazemos aligação de seu IN com o OUT do FormattedText.

### Adição da imagem estilo DVD Bounce

Nesta etapa colocamos uma imagem no visor o relógio e criamos um efeito de bounce. A imagem ai andando em diagonal e ao encontrar uma extremidade ela muda sua rota, parecido com o símbolo de DVD.

Primeiramente no display **ILI9341** adicionamos um campo do tipo Bitmap. Nele adicionamos a imagem no campo Bitmap, setamos o X como 100 e o Y como 170 para imagem começar no meio do visor e clicamos nos pins do X e do Y para poder editar seus valores posteriormente.

![Arduino%20Visuino%20Rel%20gio%20no%20display/Untitled%204.png](Arduino%20Visuino%20Rel%20gio%20no%20display/Untitled%204.png)

Para fazer a imagem andar criamos uma estrutura com os seguintes componentes:

- Counter (2x)
- Pulse Generator
- Conter (3x)

No primeiro Counter setamos seu Max como 190 e seu Min como 5, que seriam as extremidades da tela. 

Nos compares vemos se o valor vindo do Counter é igual 190 e no outro vemos se é igual a 5. caso for lançamos um tick para o outro Counter, que vai de 0 a 1. 

Esse counter é ligado a outro compare que ve se o valor é 1 e manda um tick para o atributo **reversed** do primeiro counter, fazendo então com que a imagem começe a ir para o outro lado.

Com isso pronto enviamos o outro do primeiro compare para o X do bitmap do visor.

![Arduino%20Visuino%20Rel%20gio%20no%20display/Untitled%205.png](Arduino%20Visuino%20Rel%20gio%20no%20display/Untitled%205.png)

Duplicamos todo os componentes anteriores e apenas mudamos seus valores para os extremos da alura da tela, alterando onde era 190 para 300.

Com isso temos nosso relógio com uma imagem andando pela tela.

![Arduino%20Visuino%20Rel%20gio%20no%20display/relogio-sorr.jpg](Arduino%20Visuino%20Rel%20gio%20no%20display/relogio-sorr.jpg)

Feito por: Bruno Vigentas, Gabriel Panca, Gustavo Westarb & José Henrique Teixeira.