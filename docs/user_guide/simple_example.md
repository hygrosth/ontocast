# Simple Example: From Text to Triples

This tutorial will guide you through a simple example of using OntoCast to extract semantic triples from a short piece of text. We will use the `/process` endpoint of the running OntoCast server.


## Step 1, Alternative A: Server

Before you process text, make sure you have started the OntoCast server. You can do this by running the following command from the root of the project, replacing the directory paths with your own:

```bash
uv run serve --ontology-directory "path/to/your/ontology/dir" --working-directory "path/to/your/working/dir"
```

With the server running, we can now send some text to be processed. We'll use a simple sentence about Albert Einstein.

Open a new terminal and use `curl` to send a POST request to the `http://localhost:8999/process` endpoint. The default port is `8999`.

```cmd
curl -X POST http://localhost:8999/process -H "Content-Type: application/json" -d @./examples/einstein.json
```

## Step 1, Alternative B: No server

```cmd
uv run serve --input-path "./examples/" --working-directory "C:/data/temp/ontocast/working/" --ontology-directory "C:/data/temp/ontocast/ontology/"
```

## Step 2: Understanding the Output

OntoCast will process the text and return a JSON response. The response will contain the extracted knowledge in the form of semantic triples, serialized in the Turtle (`.ttl`) format.

The expected output will look something like this (the exact output may vary slightly):

```json
{
  "facts": "@prefix ns1: <http://example.org/ontology#> .\n\nns1:Albert_Einstein a ns1:TheoreticalPhysicist ;\n    ns1:birthPlace ns1:Germany ;\n    ns1:developed ns1:Theory_of_Relativity .",
  "ontology": "@prefix owl: <http://www.w3.org/2002/07/owl#> .\n@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .\n\n<http://example.org/ontology#>\n    a owl:Ontology .\n\n<http://example.org/ontology#TheoreticalPhysicist> a rdfs:Class .\n\n<http://example.org/ontology#birthPlace> a owl:ObjectProperty .\n\n<http://example.org/ontology#developed> a owl:ObjectProperty .",
  "metadata": {}
}
```

### What does this mean?

*   **`facts`**: This contains the extracted information about entities and their relationships from your text. In our example:
    *   `Albert_Einstein` is identified as a `TheoreticalPhysicist`.
    *   His `birthPlace` was `Germany`.
    *   He `developed` the `Theory_of_Relativity`.
*   **`ontology`**: This is the schema or data model that OntoCast generated or selected to structure the facts. It defines the types of things that can exist (`Classes` like `TheoreticalPhysicist`) and the relationships they can have (`ObjectProperty` like `birthPlace` and `developed`).

This example demonstrates how OntoCast takes unstructured text, understands the concepts and relationships within it, and outputs structured, machine-readable data.

## Next Steps

Now that you have a basic understanding, you can try:
*   Processing a more complex text.
*   Processing a document (like a PDF).
*   Exploring the generated Turtle files in the working directory you specified when starting the server. 