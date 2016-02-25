# Einführung in TDD mit Java

Anhand eines kleinen Beispiels in Java möchte ich eine kurze Einführung in das Thema _Test Driven Development_ geben.

Generelles Problem bei _kurzen_ oder _kleinen_ Beispielen ist natürlich, dass sie immer recht abstrakt sind und nur sehr wenig mit dem "echten Leben" zu tun haben. Daher werde ich später auch noch ein umfangreicheres Beispiel nachliefern. An dieser Stelle muss das Abstrakte allerdings genügen.

_Test Driven Development ( TDD)_ bezeichnet eine Methodik, bei der ich mein Projekt testgetrieben, d.h. durch eine Erweiterung meiner Testfälle motiviert, voran treibe.

Als erstes betrachte ich meine Projektanforderungen und versuche mir daraus entsprechende Testfälle abzuleiten. Es empfiehlt sich, dabei nicht zu viele Tests auf einmal aufzustellen, sondern iterativ den Umfang zu erhöhen.

Als Beispiel soll mein Programm Zahlen über den Startaufruf übergeben bekommen und diese Zahlen miteinander multiplizieren. Die Herausforderung dabei ist, dass mein Kommandozeilenargument in Java nicht direkt als primitiver Zahlenwert ankommt, sondern als Zeichenkette (String). Daher muss eine Lösung gefunden werden, die Eingaben in primitive Zahlenwerte umzuwandeln und dann damit zu rechnen (was selbstverständlich kein Problem ist, aber es geht ja auch nicht um das Ziel, sondern um den Weg).

Abstrakt sehen meine Testfälle so aus.

<table border>
  <tr>
    <th>Eingabe</th>
    <th>Ausgabe</th>
  </tr>
  <tr>
    <td>""</td>
    <td>0</td>
  </tr>
  <tr>
    <td>" "</td>
    <td>0</td>
  </tr>
  <tr>
    <td>"0"</td>
    <td>0</td>
  </tr>
  <tr>
    <td>"1"</td>
    <td>1</td>
  </tr>
  <tr>
    <td>" 1"</td>
    <td>1</td>
  </tr>
  <tr>
    <td>"0,1"</td>
    <td>0</td>
  </tr>
  <tr>
    <td>"1,1"</td>
    <td>1</td>
  </tr>
  <tr>
    <td>"1,2"</td>
    <td>2</td>
  </tr>
  <tr>
    <td>"2,2"</td>
    <td>4</td>
  </tr>
  <tr>
    <td>"1,2,3"</td>
    <td>6</td>
  </tr>
</table>

Jetzt habe ich eine Basis, um ganz konkret an die Implementierung meiner Anwendung zu gehen. Meine Vorgehensweise folgt dabei immer einem ganz speziellen Zyklus.

![TDD-Zyklus]

Um überhaupt einen kompilierenden ersten Testfall zu schreiben, muss ich natürlich die zu testende Klasse sowie die entsprechende öffentliche Methode anlegen. Das ist der einzige Schritt, den ich vor dem Schreiben meiner Testfälle vornehme.

```java
public class StringCalculator {
  public int multiply(String input) {
    return 0;
  }
}
```

Somit habe ich einen Startpunkt, gegen den ich testen kann. Nun implementiere ich meinen ersten Test.

```java
public class StringCalculatorTest {
  @Test
  public void testMultiply() {
    StringCalculator calc = new StringCalculator();

    int result = calc.multiply("");

    assertEquals(0, result);
  }
}
```

Ich kann nun meinen Test laufen lassen (der zur Kompilierung nötige `StringCalculator` ist ja vorhanden), erhalte ich den _bösen roten Balken_.

![Roter Balken]

Erste Priorität hat ab sofort immer die Umfärbung des bösen roten Balkens in den guten grünen Balken!
