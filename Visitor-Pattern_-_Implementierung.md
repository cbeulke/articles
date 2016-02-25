# Visitor-Pattern - Implementierung

Das Visitor-Pattern ist grundlegend für echte objektorientierte Arbeit. Es dient der Unterscheidung verschiedener Ausprägungen eines Objektes, ohne auf unsichere Hilfsmittel wie beispielsweise `instanceof` und Typecasting in Java zurückgreifen zu müssen.

Die Struktur eines einfachen Beispiels sieht wie folgt aus.

![Hierarchie Person mit Visitor](/content/images/2015/05/class_visitor.png)

Zuerst deklariere ich das Parent-Objekt einer beliebigen Hierarchie.

    public abstract class Person {
        public abstract void accept(PersonVisitor visitor);
    }

Es ist entscheidend, dass dieses Objekt `abstract` ist. Dadurch wird garantiert, dass abgeleitete Objekte auch auf den Visitor reagieren können.

Als nächstes leite ich zwei Klassen von `Person` ab.

    public class Employee extends Person {
        public void accept(PersonVisitor visitor) {
            visitor.handle(this);
        }
    }

    public class Customer extends Person {
        public void accept(PersonVisitor visitor) {
            visitor.handle(this);
        }
    }

Diese Klassen sehen hier bis auf den Bezeichner identisch aus, was auch durchaus ausreichend ist. Selbstverständlich können die verschiedenen Ausprägungen von `Person` auch noch weitere, unterschiedliche Eigenschaften und Methoden beinhalten, dies geht jedoch über den grundlegenden Einsatzzweck des Visitor-Patterns hinaus.

Als letztes strukturelles Element deklariere ich das Visitor-Interface.

    public interface PersonVisitor {
        public void handle(Employee visitee);
        public void handle(Customer visitee);
    }

Mit `handle()` kann überall dort, wo eine unterschiedliche Behandlung der verschiedenen Ausprägungen von `Person` verlangt wird diese an der entsprechenden Stelle implementieren.

Ein Anwendungsbeispiel sieht wie folgt aus.

    persons.add(new Employee());
    persons.add(new Customer());
    persons.add(new Employee());
    persons.add(new Customer());

    for(Person person : persons) {
        person.accept(new PersonVisitor() {
            public void handle(Customer visitee) {
                System.out.println("Ich bin ein Kunde...");
            }

            public void handle(Employee visitee) {
                System.out.println("Ich bin ein Mitarbeiter!");
            }
        });
    }

Das Ergebnis ist folgende Ausgabe.

    $ Ich bin ein Mitarbeiter!
    $ Ich bin ein Kunde...
    $ Ich bin ein Mitarbeiter!
    $ Ich bin ein Kunde...

Natürlich lassen sich in den Implementierungen von `handle()` auch komplexere Sachverhalte abbilden.

Der eigentliche Vorteil wird aber dann sichtbar, wenn die Ausprägungen von `Person` in ihrer jeweiligen Detailierung benötigt werden. Der Zugriff auf individuelle Eigenschaften und Methoden ist dann durch die Überladung von `handle()` möglich, da hier die jeweilige konkrete Ausprägung zur Verfügung steht.

    public class Employee extends Person {
        ...
        public void makeCoffee() {
            System.out.println("*Blubber*");
        }
    }

    public class Customer extends Person {
        private Date birthday;
        ...
        public Date getBirthday() {
            return birthday;
        }
    }

    persons.add(new Employee());
    persons.add(new Customer());

    for(Person person : persons) {
        person.accept(new PersonVisitor() {
            public void handle(Customer visitee) {
                System.out.println(
                    "Ich bin ein Kunde und habe am " + 
                    visitee.getBirthday() + 
                    " Geburtstag (und ich weiss, dass" + 
                    " das Datum schlecht formatiert ist)...");
            }

            public void handle(Employee visitee) {
                System.out.println("Ich bin Mitarbeiter und ganz fleissig!");
                visitee.makeCooffee();
            }
        });
    }

Das Ergebnis sieht dann folgendermaßen aus.

    $ Ich bin ein Kunde und habe am 1980-01-01T00:00:00.000+00:53:28 Geburtstag (und ich weiss, dass das Datum schlecht formatiert ist)...
    $ Ich bin ein Mitarbeiter und ganz fleissig!
    $ *Blubber*