# Model-Driven Apps: Built on Dataverse, Component-Focused

- Model-Driven Apps are built on Microsoft Dataverse, a secure, cloud-based data platform that stores business data in tables with defined relationships and metadata [web:1][web:2][web:4].
- These apps apply a data-first approach, where the app UI is automatically generated based on the underlying data model and business logic, reducing the need for manual UI design [web:1][web:2].
- Best suited for complex business processes and sophisticated scenarios that require strong data integrity, business rules, and workflows [web:1][web:3].
  
> [!IMPORTANT]  
> Model-Driven Apps require a well-structured Dataverse data model with entities (tables), fields (columns), and relationships defined before app creation.

- The app layout is component-focused, using forms, views, charts, dashboards, and business process flows configured through a visual app designer [web:7][web:3].
- UI components are mostly pre-built and responsive by design, ensuring consistency and adaptability across devices without extensive design effort [web:1][web:2].
- Business logic including business rules, workflows, and automation can be embedded directly within Dataverse and the app, enabling automation and validation [web:1][web:4].

> [!TIP]  
> Leverage security roles in Dataverse to manage permissions, controlling who can view, create, or update specific data entities within the app [web:2].

- Model-Driven Apps automatically render user interfaces that adapt to various screen sizes including desktop and mobile devices [web:1].
- Suitable for scenarios like customer relationship management (CRM), finance, operations where complex data relationships and processes are central [web:3].
- Supports integration with other Power Platform services like Power Automate for workflows and Power BI for analytics [web:4].

> [!NOTE]  
> Compared to Canvas Apps, Model-Driven Apps offer less UI customization but provide a faster way to build apps relying on business data structure and logic.

- Development flow revolves around:
  - Modeling business data and relationships in Dataverse
  - Defining business processes and rules
  - Configuring app components in the app designer
  - Assigning security roles and publishing the app [web:3].

> [!WARNING]  
> Model-Driven Apps are not ideal if highly custom UI or pixel-perfect design is required.

- Custom pages built using Canvas app designer can be included within Model-Driven Apps for mixed UI flexibility [web:3].
- Common uses include managing leads, opportunities, customer data, service requests, expense reporting, and other data-centric business operations [web:3][web:9].
