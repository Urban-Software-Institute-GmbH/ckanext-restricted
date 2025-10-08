## 1.0.1 (2025-10-08)

- Release that is compatible with **CKAN version 2.11.3**
- Fix for handling the case where dataset IDs are found in solr but have been removed from DB. The loop continues even if some of the IDs are raising the exception of Not Found in package_search.

## 1.0.0 (2025-09-11)

Release that is compatible with **CKAN version 2.11.3**

### Refactor
- update deprecated methods
- remove unsupported methods from template
- update user handling
- remove unused imports

## 0.0.5 (2025-09-11)

Release that is compatible with **CKAN version 2.9.9b**