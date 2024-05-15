```mermaid
erDiagram
	TemplateNucleus {
		UUID id PK
		UUID blueprint_nucleus_id FK
	}

	TemplateEditableFields {
		
	}

	TemplateSavedConfig {
		UUID id
		String value
	}

	TemplateSavedConfigValues {
		
	}
```