# Visitor-Pattern - Erweiterung

Eine Hierarchie zu erweitern ist nicht schwierig. Ich schaue mir eine Eltern-Klasse an, suche mir davon abgeleitete Klassen heraus und erstelle nach Bedarf neue Ableitungen - soweit so gut.

Falls nun aber diese neuen Ableitungen auch in ihrem individuellen Verhalten dort berücksichtigt werden müssen, wo bereits vorhandene "Geschwister" zum Einsatz kommen, kann dies schnell im Chaos enden.

Um diesem Chaos vorzubeugen kommt das Visitor-Pattern zum Einsatz, dessen Implementierung ich an anderer Stelle bereits erläutert habe. Hier möchte ich nun auf die konkreten Schritte bei der Erweiterung einer Struktur unter Zuhilfenahme dieses Patterns eingehen.

Ich besitze bereits die abstrakte Klasse `Person` sowie die davon abgeleiteten Klassen `Employee` und `Customer`. Als nächstes möchte ich die neue Klasse `Intern`einführen, welche Praktikanten in meinem Betrieb darstellen soll.

Zunächst deklariere ich die Klasse `Intern`.

    public class Intern {
    }

Als nächstes möchte ich bekanntermaßen von der Klasse `Person` ableiten. Diese schreibt mir jedoch vor, dass ich die Methode `accept(PersonVisitor visitor)` implementiere.

    public class Intern extends Person {
        public void accept(PersonVisitor visitor) {
            // TODO Implementierung!
        }
    }

Bis hierhin kompiliert mein Programm wieder, jedoch habe ich nun ein offenes `TODO`. Dieses fülle ich selbstverständlich sofort, der eingesetzte Code ist immer gleich.

    public class Intern extends Person {
        public void accept(PersonVisitor visitor) {
            visitor.handle(this);
        }
    }

Nun kompiliert mein Programm _nicht_ mehr, da der `PersonVisitor`die Überladung der Methode `handle()` nicht mit der Klasse `Intern` kennt. Dies ändere ich natürlich sofort.

    public interface PersonVisitor {
        public void handle(Employee visitee);
        public void handle(Customer visitee);
        public void handle(Intern visitee); // Das ist neu!
    }

Abschließend muss ich nun noch überall dort, wo ich den `PersonVisitor`eingesetzt habe die entsprechend neue Überladung implementieren. Dies kann ganz schön aufwendig sein, falls der Visitor an vielen Stellen im Einsatz ist. Jedoch werde ich so von meinem eigenen Code dazu gezwungen, diese Stellen auch ausnahmslos anzusehen und zu berücksichtigen ohne dass ich einen Einsatzort vergesse.