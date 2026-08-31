# moqui-demo

Small data and settings component used on [demo.moqui.org](https://demo.moqui.org).

This is a **public demo** lock-down: keep Marble ERP / HiveMind / PopCommerce / Example fully usable (including writes) with the well-known `john.doe` login, but take away RCE, SQL, bulk import/export, file manager, and write-admin on Tools/System.

It is **not** production policy (production: no demo data, don’t put Tools on the public hostname). See [SECURITY_SURFACE.md](https://github.com/moqui/moqui-framework/blob/master/SECURITY_SURFACE.md).

## What applies when

- **`MoquiConf.xml`** always merges when this component is present: `/groovysh` disabled; sharp Tools/System screens replaced with `Disabled.xml`.
- **`data/MoquiDemoServerData.xml`** is `type="demo"`: loaded only with demo data. Overwrites seed authz so `ADMIN` is VIEW-only on Tools, System, `/rest/s1/moqui`, EntitySync services, and SystemMessage receive; thru-dates `john.doe` out of `ADMIN_ADV`; thru-dates `ElasticRemote` / `KibanaRemote` on `ADMIN`.

Both are required for the full demo recipe.

## Stubbed screens

GroovyShell, DataImport, DataExport, DataSnapshot, SpeedTest, SqlRunner, SqlScriptRunner, ServiceRun, ServiceLoadRunner, ElFinder.

Cache, Instance, AutoScreen, DataEdit, and the Tools/System dashboards stay mounted. Mutating transitions are `update`/`all` in moqui-runtime, so VIEW-only inherit returns 403 (and hides links that check `isPermitted`).

`/notws` and `/fop/*` stay enabled for SPA notifications and PDF.

## Manual checks

- GroovyShell URL and `/groovysh` do not execute.
- DataImport / DataExport / ServiceRun / ElFinder / SQL Runner show “This screen is disabled.”
- AutoFind still renders; Marble (and other apps) still create/update.
- `/elastic/*` is 403 for `john.doe`.
