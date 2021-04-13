# Micro Frontends
- Separate loosely coupled components of the UI that are developed applying the principles of microservices on the frontend;
- The application is split into vertical slices, which goes end to end from the UI to the DB;
- They are very popular with e-commerce websites;
- Each team can own a dedicated page or small components that are integrated into other pages, these components are known as **_fragments_**;
### Why Microfrontends
- For full-stack teams, owning an entire service from end to end is easier than having dedicated frontend teams;
- Can leverage different technologies;
- Fit for medium to large websites, won't make anything simpler in case of simple apps;

# Microfrontends Integration
- Client-side integration;
- Server-side integration;

### Client-Side Integration
- Basic links
  - Microfrontends can integrated via basic links within a page via iframes;
  - Not good!
- Web Components & SPAs (recommended)

### Server-Side Integration
- Uses server-side rendering;
- The complete pre-built page of the website is delivered to the client from the server;
- Cuts down the loading time, since the browser doesn't have to do any heavy lifting;