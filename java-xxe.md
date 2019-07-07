<b> XXE </b> <br>

<b>Mitigation </b>
JAXB

You can prevent the Xml eXternal Entity (XXE) attack by unmarshalling from an XMLStreamReader that has the IS_SUPPORTING_EXTERNAL_ENTITIES and/or XMLInputFactory.SUPPORT_DTD properties set to false.

JAX-WS

A JAX-WS implementation should take care of this for you. If it doesn't I would recommend opening a bug against the specific implmententation.

EXAMPLE

Demo

package xxe;

import javax.xml.bind.*;
import javax.xml.stream.*;
import javax.xml.transform.stream.StreamSource;

public class Demo {

    public static void main(String[] args) throws Exception {
        JAXBContext jc = JAXBContext.newInstance(Customer.class);

        XMLInputFactory xif = XMLInputFactory.newFactory();
<b><i>  xif.setProperty(XMLInputFactory.IS_SUPPORTING_EXTERNAL_ENTITIES, false); </b></i>
        <b><i>xif.setProperty(XMLInputFactory.SUPPORT_DTD, false); </b></i>
        XMLStreamReader xsr = xif.createXMLStreamReader(new StreamSource("src/xxe/input.xml"));

        Unmarshaller unmarshaller = jc.createUnmarshaller();
        Customer customer = (Customer) unmarshaller.unmarshal(xsr);

        Marshaller marshaller = jc.createMarshaller();
        marshaller.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
        marshaller.marshal(customer, System.out);
    }

}
