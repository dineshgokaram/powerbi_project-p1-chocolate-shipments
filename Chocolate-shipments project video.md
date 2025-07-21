+---------------------+
|      Processor      |
+---------------------+
| - mode: IDataMode                 |
| - database: IDatabaseConnector   |
+---------------------+
| +configure(modeId: ModeIdentifier, dbId: DatabaseIdentifier): void |
| +process(data: DataPoint): void                                     |
+---------------------+

+---------------------+           +--------------------------+
|    IDataMode        |<interface>|   IDatabaseConnector     |<interface>
+---------------------+           +--------------------------+
| +execute(data: DataPoint): void | +connect(): void         |
|                                 | +insert(data: DataPoint): void  |
|                                 | +validate(data: DataPoint): boolean |
+---------------------+           +--------------------------+

          ▲
          |
+----------------------+
|     DumpMode         |
+----------------------+
| +execute(data): void |
+----------------------+

          ▲
          |
+--------------------------+
|     PassthroughMode      |
+--------------------------+
| +execute(data): void     |
+--------------------------+

          ▲
          |
+--------------------------+
|     ValidateMode         |
+--------------------------+
| +execute(data): void     |
+--------------------------+

                         ▲                 ▲                  ▲
                         |                 |                  |
            +---------------------+ +------------------+ +------------------+
            | PostgresConnector   | | RedisConnector    | | ElasticConnector |
            +---------------------+ +------------------+ +------------------+
            | +connect()          | | +connect()        | | +connect()       |
            | +insert(data)       | | +insert(data)     | | +insert(data)    |
            | +validate(data)     | | +validate(data)   | | +validate(data)  |
            +---------------------+ +------------------+ +------------------+

Legend:
- `IDataMode` determines how a DataPoint is processed.
- `IDatabaseConnector` abstracts DB interactions.
- `Processor.configure(...)` changes mode/database at runtime.
