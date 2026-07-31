---
name: Fetch Orakl Oncology website page content
description: >-
  Retrieve rendered page/content from the Orakl Oncology public website API
  (API Platform / Roadiz Hydra + JSON-LD) — resolve a page by its front-end
  path, or browse pages, tags and folders as paginated collections.
api: openapi/orakl-website-openapi-original.json
operations:
  - page_get_by_path
  - page_get_collection
  - api_tags_get_collection
  - api_folders_get_collection
  - getCommonContent
---

# Fetch Orakl Oncology website page content

Read-only content retrieval from the Orakl Oncology public website API. All
responses are Hydra + JSON-LD (`application/ld+json`).

## Auth
Public read endpoints generally require no token. If you receive `401`, obtain a
JWT via `POST /api/token` (`{username, password}`) and send it as
`Authorization: Bearer <jwt>` (scheme `JWT`, bearerFormat JWT). See
`authentication/orakl-authentication.yml`.

## Steps
1. To resolve a specific page, call `page_get_by_path`
   (`GET /api/web_response_by_path`) with the front-end path — returns the
   composed web response for that path.
2. To browse pages, call `page_get_collection` (`GET /api/pages`). Paginate with
   `page` and `itemsPerPage` (default 15, max 50). Read `hydra:member` for items
   and `hydra:view` for next/previous links.
3. Use `api_tags_get_collection` (`GET /api/tags`) and
   `api_folders_get_collection` (`GET /api/folders`) to discover taxonomy and
   folder structure for filtering.
4. Use `getCommonContent` (`GET /api/common_content`) for shared site content
   (header/footer/common blocks).

## Conventions
- Trim payloads with sparse fieldsets: `properties[]=name&properties[]=slug`.
- Pagination is page-number based (`page`, `itemsPerPage`). See
  `conventions/orakl-conventions.yml`.
- Errors are `application/problem+json` (RFC 9457): `400` bad request, `401`
  unauthorized, `404` not found, `422` validation. See
  `errors/orakl-problem-types.yml`.
