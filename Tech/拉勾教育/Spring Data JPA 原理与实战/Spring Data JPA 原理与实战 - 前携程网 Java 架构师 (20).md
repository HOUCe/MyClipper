---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=490#/detail/pc?id=4700
author: 
---

# [Spring Data JPA 原理与实战 - 前携程网 Java 架构师 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=490#/detail/pc?id=4700)


`

1.  String JPA_PERSISTENCE_PROVIDER = "javax.persistence.provider";
    
2.  String JPA_TRANSACTION_TYPE = "javax.persistence.transactionType";
    
3.  String JPA_JTA_DATASOURCE = "javax.persistence.jtaDataSource";
    
4.  String JPA_NON_JTA_DATASOURCE = "javax.persistence.nonJtaDataSource";
    
5.  String JPA_JDBC_DRIVER = "javax.persistence.jdbc.driver";
    
6.  String JPA_JDBC_URL = "javax.persistence.jdbc.url";
    
7.  String JPA_JDBC_USER = "javax.persistence.jdbc.user";
    
8.  String JPA_JDBC_PASSWORD = "javax.persistence.jdbc.password";
    
9.  String JPA_SHARED_CACHE_MODE = "javax.persistence.sharedCache.mode";
    
10.  String JPA_SHARED_CACHE_RETRIEVE_MODE ="javax.persistence.cache.retrieveMode";
    
11.  String JPA_SHARED_CACHE_STORE_MODE ="javax.persistence.cache.storeMode";
    
12.  String JPA_VALIDATION_MODE = "javax.persistence.validation.mode";
    
13.  String JPA_VALIDATION_FACTORY = "javax.persistence.validation.factory";
    
14.  String JPA_PERSIST_VALIDATION_GROUP = "javax.persistence.validation.group.pre-persist";
    
15.  String JPA_UPDATE_VALIDATION_GROUP = "javax.persistence.validation.group.pre-update";
    
16.  String JPA_REMOVE_VALIDATION_GROUP = "javax.persistence.validation.group.pre-remove";
    
17.  String JPA_LOCK_SCOPE = "javax.persistence.lock.scope";
    
18.  String JPA_LOCK_TIMEOUT = "javax.persistence.lock.timeout";
    
19.  String CDI_BEAN_MANAGER = "javax.persistence.bean.manager";
    
20.  String CLASSLOADERS = "hibernate.classLoaders";
    
21.  String TC_CLASSLOADER = "hibernate.classLoader.tccl_lookup_precedence";
    
22.  String APP_CLASSLOADER = "hibernate.classLoader.application";
    
23.  String RESOURCES_CLASSLOADER = "hibernate.classLoader.resources";
    
24.  String HIBERNATE_CLASSLOADER = "hibernate.classLoader.hibernate";
    
25.  String ENVIRONMENT_CLASSLOADER = "hibernate.classLoader.environment";
    
26.  String JPA_METAMODEL_GENERATION = "hibernate.ejb.metamodel.generation";
    
27.  String JPA_METAMODEL_POPULATION = "hibernate.ejb.metamodel.population";
    
28.  String STATIC_METAMODEL_POPULATION = "hibernate.jpa.static_metamodel.population";
    
29.  String CONNECTION_PROVIDER ="hibernate.connection.provider_class";
    
30.  String DRIVER ="hibernate.connection.driver_class";
    
31.  String URL ="hibernate.connection.url";
    
32.  String USER ="hibernate.connection.username";
    
33.  String PASS ="hibernate.connection.password";
    
34.  String ISOLATION ="hibernate.connection.isolation";
    
35.  String AUTOCOMMIT = "hibernate.connection.autocommit";
    
36.  String POOL_SIZE ="hibernate.connection.pool_size";
    
37.  String DATASOURCE ="hibernate.connection.datasource";
    
38.  String CONNECTION_PROVIDER_DISABLES_AUTOCOMMIT= "hibernate.connection.provider_disables_autocommit";
    
39.  String CONNECTION_PREFIX = "hibernate.connection";
    
40.  String JNDI_CLASS ="hibernate.jndi.class";
    
41.  String JNDI_URL ="hibernate.jndi.url";
    
42.  String JNDI_PREFIX = "hibernate.jndi";
    
43.  String DIALECT ="hibernate.dialect";
    
44.  String DIALECT_RESOLVERS = "hibernate.dialect_resolvers";
    
45.  String STORAGE_ENGINE = "hibernate.dialect.storage_engine";
    
46.  String SCHEMA_MANAGEMENT_TOOL = "hibernate.schema_management_tool";
    
47.  String TRANSACTION_COORDINATOR_STRATEGY = "hibernate.transaction.coordinator_class";
    
48.  String JTA_PLATFORM = "hibernate.transaction.jta.platform";
    
49.  String PREFER_USER_TRANSACTION = "hibernate.jta.prefer_user_transaction";
    
50.  String JTA_PLATFORM_RESOLVER = "hibernate.transaction.jta.platform_resolver";
    
51.  String JTA_CACHE_TM = "hibernate.jta.cacheTransactionManager";
    
52.  String JTA_CACHE_UT = "hibernate.jta.cacheUserTransaction";
    
53.  String JDBC_TYLE_PARAMS_ZERO_BASE = "hibernate.query.sql.jdbc_style_params_base";
    
54.  String DEFAULT_CATALOG = "hibernate.default_catalog";
    
55.  String DEFAULT_SCHEMA = "hibernate.default_schema";
    
56.  String DEFAULT_CACHE_CONCURRENCY_STRATEGY = "hibernate.cache.default_cache_concurrency_strategy";
    
57.  String USE_NEW_ID_GENERATOR_MAPPINGS = "hibernate.id.new_generator_mappings";
    
58.  String FORCE_DISCRIMINATOR_IN_SELECTS_BY_DEFAULT = "hibernate.discriminator.force_in_select";
    
59.  String IMPLICIT_DISCRIMINATOR_COLUMNS_FOR_JOINED_SUBCLASS = "hibernate.discriminator.implicit_for_joined";
    
60.  String IGNORE_EXPLICIT_DISCRIMINATOR_COLUMNS_FOR_JOINED_SUBCLASS = "hibernate.discriminator.ignore_explicit_for_joined";
    
61.  String USE_NATIONALIZED_CHARACTER_DATA = "hibernate.use_nationalized_character_data";
    
62.  String SCANNER_DEPRECATED = "hibernate.ejb.resource_scanner";
    
63.  String SCANNER = "hibernate.archive.scanner";
    
64.  String SCANNER_ARCHIVE_INTERPRETER = "hibernate.archive.interpreter";
    
65.  String SCANNER_DISCOVERY = "hibernate.archive.autodetection";
    
66.  String IMPLICIT_NAMING_STRATEGY = "hibernate.implicit_naming_strategy";
    
67.  String PHYSICAL_NAMING_STRATEGY = "hibernate.physical_naming_strategy";
    
68.  String ARTIFACT_PROCESSING_ORDER = "hibernate.mapping.precedence";
    
69.  String KEYWORD_AUTO_QUOTING_ENABLED = "hibernate.auto_quote_keyword";
    
70.  String XML_MAPPING_ENABLED = "hibernate.xml_mapping_enabled";
    
71.  String SESSION_FACTORY_NAME = "hibernate.session_factory_name";
    
72.  String SESSION_FACTORY_NAME_IS_JNDI = "hibernate.session_factory_name_is_jndi";
    
73.  String SHOW_SQL ="hibernate.show_sql";
    
74.  String FORMAT_SQL ="hibernate.format_sql";
    
75.  String USE_SQL_COMMENTS ="hibernate.use_sql_comments";
    
76.  String MAX_FETCH_DEPTH = "hibernate.max_fetch_depth";
    
77.  String DEFAULT_BATCH_FETCH_SIZE = "hibernate.default_batch_fetch_size";
    
78.  String USE_STREAMS_FOR_BINARY = "hibernate.jdbc.use_streams_for_binary";
    
79.  String USE_SCROLLABLE_RESULTSET = "hibernate.jdbc.use_scrollable_resultset";
    
80.  String USE_GET_GENERATED_KEYS = "hibernate.jdbc.use_get_generated_keys";
    
81.  String STATEMENT_FETCH_SIZE = "hibernate.jdbc.fetch_size";
    
82.  String STATEMENT_BATCH_SIZE = "hibernate.jdbc.batch_size";
    
83.  String BATCH_STRATEGY = "hibernate.jdbc.factory_class";
    
84.  String BATCH_VERSIONED_DATA = "hibernate.jdbc.batch_versioned_data";
    
85.  String JDBC_TIME_ZONE = "hibernate.jdbc.time_zone";
    
86.  String AUTO_CLOSE_SESSION = "hibernate.transaction.auto_close_session";
    
87.  String FLUSH_BEFORE_COMPLETION = "hibernate.transaction.flush_before_completion";
    
88.  String ACQUIRE_CONNECTIONS = "hibernate.connection.acquisition_mode";
    
89.  String RELEASE_CONNECTIONS = "hibernate.connection.release_mode";
    
90.  String CONNECTION_HANDLING = "hibernate.connection.handling_mode";
    
91.  String CURRENT_SESSION_CONTEXT_CLASS = "hibernate.current_session_context_class";
    
92.  String USE_IDENTIFIER_ROLLBACK = "hibernate.use_identifier_rollback";
    
93.  String USE_REFLECTION_OPTIMIZER = "hibernate.bytecode.use_reflection_optimizer";
    
94.  String ENFORCE_LEGACY_PROXY_CLASSNAMES = "hibernate.bytecode.enforce_legacy_proxy_classnames";
    
95.  String ALLOW_ENHANCEMENT_AS_PROXY = "hibernate.bytecode.allow_enhancement_as_proxy";
    
96.  String QUERY_TRANSLATOR = "hibernate.query.factory_class";
    
97.  String QUERY_SUBSTITUTIONS = "hibernate.query.substitutions";
    
98.  String QUERY_STARTUP_CHECKING = "hibernate.query.startup_check";
    
99.  String CONVENTIONAL_JAVA_CONSTANTS = "hibernate.query.conventional_java_constants";
    
100.  String SQL_EXCEPTION_CONVERTER = "hibernate.jdbc.sql_exception_converter";
    
101.  String WRAP_RESULT_SETS = "hibernate.jdbc.wrap_result_sets";
    
102.  String NATIVE_EXCEPTION_HANDLING_51_COMPLIANCE = "hibernate.native_exception_handling_51_compliance";
    
103.  String ORDER_UPDATES = "hibernate.order_updates";
    
104.  String ORDER_INSERTS = "hibernate.order_inserts";
    
105.  String JPA_CALLBACKS_ENABLED = "hibernate.jpa_callbacks.enabled";
    
106.  String DEFAULT_NULL_ORDERING = "hibernate.order_by.default_null_ordering";
    
107.  String LOG_JDBC_WARNINGS = "hibernate.jdbc.log.warnings";
    
108.  String BEAN_CONTAINER = "hibernate.resource.beans.container";
    
109.  String C3P0_CONFIG_PREFIX = "hibernate.c3p0";
    
110.  String C3P0_MAX_SIZE = "hibernate.c3p0.max_size";
    
111.  String C3P0_MIN_SIZE = "hibernate.c3p0.min_size";
    
112.  String C3P0_TIMEOUT = "hibernate.c3p0.timeout";
    
113.  String C3P0_MAX_STATEMENTS = "hibernate.c3p0.max_statements";
    
114.  String C3P0_ACQUIRE_INCREMENT = "hibernate.c3p0.acquire_increment";
    
115.  String C3P0_IDLE_TEST_PERIOD = "hibernate.c3p0.idle_test_period";
    
116.  String PROXOOL_CONFIG_PREFIX = "hibernate.proxool";
    
117.  String PROXOOL_PREFIX = PROXOOL_CONFIG_PREFIX;
    
118.  String PROXOOL_XML = "hibernate.proxool.xml";
    
119.  String PROXOOL_PROPERTIES = "hibernate.proxool.properties";
    
120.  String PROXOOL_EXISTING_POOL = "hibernate.proxool.existing_pool";
    
121.  String PROXOOL_POOL_ALIAS = "hibernate.proxool.pool_alias";
    
122.  String CACHE_REGION_FACTORY = "hibernate.cache.region.factory_class";
    
123.  String CACHE_KEYS_FACTORY = "hibernate.cache.keys_factory";
    
124.  String CACHE_PROVIDER_CONFIG = "hibernate.cache.provider_configuration_file_resource_path";
    
125.  String USE_SECOND_LEVEL_CACHE = "hibernate.cache.use_second_level_cache";
    
126.  String USE_QUERY_CACHE = "hibernate.cache.use_query_cache";
    
127.  String QUERY_CACHE_FACTORY = "hibernate.cache.query_cache_factory";
    
128.  String CACHE_REGION_PREFIX = "hibernate.cache.region_prefix";
    
129.  String USE_MINIMAL_PUTS = "hibernate.cache.use_minimal_puts";
    
130.  String USE_STRUCTURED_CACHE = "hibernate.cache.use_structured_entries";
    
131.  String AUTO_EVICT_COLLECTION_CACHE = "hibernate.cache.auto_evict_collection_cache";
    
132.  String USE_DIRECT_REFERENCE_CACHE_ENTRIES = "hibernate.cache.use_reference_entries";
    
133.  String DEFAULT_ENTITY_MODE = "hibernate.default_entity_mode";
    
134.  String GLOBALLY_QUOTED_IDENTIFIERS = "hibernate.globally_quoted_identifiers";
    
135.  String GLOBALLY_QUOTED_IDENTIFIERS_SKIP_COLUMN_DEFINITIONS = "hibernate.globally_quoted_identifiers_skip_column_definitions";
    
136.  String CHECK_NULLABILITY = "hibernate.check_nullability";
    
137.  String BYTECODE_PROVIDER = "hibernate.bytecode.provider";
    
138.  String JPAQL_STRICT_COMPLIANCE= "hibernate.query.jpaql_strict_compliance";
    
139.  String PREFER_POOLED_VALUES_LO = "hibernate.id.optimizer.pooled.prefer_lo";
    
140.  String PREFERRED_POOLED_OPTIMIZER = "hibernate.id.optimizer.pooled.preferred";
    
141.  String QUERY_PLAN_CACHE_MAX_STRONG_REFERENCES = "hibernate.query.plan_cache_max_strong_references";
    
142.  String QUERY_PLAN_CACHE_MAX_SOFT_REFERENCES = "hibernate.query.plan_cache_max_soft_references";
    
143.  String QUERY_PLAN_CACHE_MAX_SIZE = "hibernate.query.plan_cache_max_size";
    
144.  String QUERY_PLAN_CACHE_PARAMETER_METADATA_MAX_SIZE = "hibernate.query.plan_parameter_metadata_max_size";
    
145.  String NON_CONTEXTUAL_LOB_CREATION = "hibernate.jdbc.lob.non_contextual_creation";
    
146.  String HBM2DDL_AUTO = "hibernate.hbm2ddl.auto";
    
147.  String HBM2DDL_DATABASE_ACTION = "javax.persistence.schema-generation.database.action";
    
148.  String HBM2DDL_SCRIPTS_ACTION = "javax.persistence.schema-generation.scripts.action";
    
149.  String HBM2DDL_CONNECTION = "javax.persistence.schema-generation-connection";
    
150.  String HBM2DDL_DB_NAME = "javax.persistence.database-product-name";
    
151.  String HBM2DDL_DB_MAJOR_VERSION = "javax.persistence.database-major-version";
    
152.  String HBM2DDL_DB_MINOR_VERSION = "javax.persistence.database-minor-version";
    
153.  String HBM2DDL_CREATE_SOURCE = "javax.persistence.schema-generation.create-source";
    
154.  String HBM2DDL_DROP_SOURCE = "javax.persistence.schema-generation.drop-source";
    
155.  String HBM2DDL_CREATE_SCRIPT_SOURCE = "javax.persistence.schema-generation.create-script-source";
    
156.  String HBM2DDL_DROP_SCRIPT_SOURCE = "javax.persistence.schema-generation.drop-script-source";
    
157.  String HBM2DDL_SCRIPTS_CREATE_TARGET = "javax.persistence.schema-generation.scripts.create-target";
    
158.  String HBM2DDL_SCRIPTS_DROP_TARGET = "javax.persistence.schema-generation.scripts.drop-target";
    
159.  String HBM2DDL_IMPORT_FILES = "hibernate.hbm2ddl.import_files";
    
160.  String HBM2DDL_LOAD_SCRIPT_SOURCE = "javax.persistence.sql-load-script-source";
    
161.  String HBM2DDL_IMPORT_FILES_SQL_EXTRACTOR = "hibernate.hbm2ddl.import_files_sql_extractor";
    
162.  String HBM2DDL_CREATE_NAMESPACES = "hibernate.hbm2ddl.create_namespaces";
    
163.  String HBM2DLL_CREATE_NAMESPACES = "hibernate.hbm2dll.create_namespaces";
    
164.  String HBM2DDL_CREATE_SCHEMAS = "javax.persistence.create-database-schemas";
    
165.  String HBM2DLL_CREATE_SCHEMAS = HBM2DDL_CREATE_SCHEMAS;
    
166.  String HBM2DDL_FILTER_PROVIDER = "hibernate.hbm2ddl.schema_filter_provider";
    
167.  String HBM2DDL_JDBC_METADATA_EXTRACTOR_STRATEGY = "hibernate.hbm2ddl.jdbc_metadata_extraction_strategy";
    
168.  String HBM2DDL_DELIMITER = "hibernate.hbm2ddl.delimiter";
    
169.  String HBM2DDL_CHARSET_NAME = "hibernate.hbm2ddl.charset_name";
    
170.  String HBM2DDL_HALT_ON_ERROR = "hibernate.hbm2ddl.halt_on_error";
    
171.  String JMX_ENABLED = "hibernate.jmx.enabled";
    
172.  String JMX_PLATFORM_SERVER = "hibernate.jmx.usePlatformServer";
    
173.  String JMX_AGENT_ID = "hibernate.jmx.agentId";
    
174.  String JMX_DOMAIN_NAME = "hibernate.jmx.defaultDomain";
    
175.  String JMX_SF_NAME = "hibernate.jmx.sessionFactoryName";
    
176.  String JMX_DEFAULT_OBJ_NAME_DOMAIN = "org.hibernate.core";
    
177.  String CUSTOM_ENTITY_DIRTINESS_STRATEGY = "hibernate.entity_dirtiness_strategy";
    
178.  String USE_ENTITY_WHERE_CLAUSE_FOR_COLLECTIONS = "hibernate.use_entity_where_clause_for_collections";
    
179.  String MULTI_TENANT = "hibernate.multiTenancy";
    
180.  String MULTI_TENANT_CONNECTION_PROVIDER = "hibernate.multi_tenant_connection_provider";
    
181.  String MULTI_TENANT_IDENTIFIER_RESOLVER = "hibernate.tenant_identifier_resolver";
    
182.  String INTERCEPTOR = "hibernate.session_factory.interceptor";
    
183.  String SESSION_SCOPED_INTERCEPTOR = "hibernate.session_factory.session_scoped_interceptor";
    
184.  String STATEMENT_INSPECTOR = "hibernate.session_factory.statement_inspector";
    
185.  String ENABLE_LAZY_LOAD_NO_TRANS = "hibernate.enable_lazy_load_no_trans";
    
186.  String HQL_BULK_ID_STRATEGY = "hibernate.hql.bulk_id_strategy";
    
187.  String BATCH_FETCH_STYLE = "hibernate.batch_fetch_style";
    
188.  String DELAY_ENTITY_LOADER_CREATIONS = "hibernate.loader.delay_entity_loader_creations";
    
189.  String JTA_TRACK_BY_THREAD = "hibernate.jta.track_by_thread";
    
190.  String JACC_CONTEXT_ID = "hibernate.jacc_context_id";
    
191.  String JACC_PREFIX = "hibernate.jacc";
    
192.  String JACC_ENABLED = "hibernate.jacc.enabled";
    
193.  String ENABLE_SYNONYMS = "hibernate.synonyms";
    
194.  String EXTRA_PHYSICAL_TABLE_TYPES = "hibernate.hbm2ddl.extra_physical_table_types";
    
195.  String DEPRECATED_EXTRA_PHYSICAL_TABLE_TYPES = "hibernate.hbm2dll.extra_physical_table_types";
    
196.  String UNIQUE_CONSTRAINT_SCHEMA_UPDATE_STRATEGY = "hibernate.schema_update.unique_constraint_strategy";
    
197.  String GENERATE_STATISTICS = "hibernate.generate_statistics";
    
198.  String LOG_SESSION_METRICS = "hibernate.session.events.log";
    
199.  String LOG_SLOW_QUERY = "hibernate.session.events.log.LOG_QUERIES_SLOWER_THAN_MS";
    
200.  String AUTO_SESSION_EVENTS_LISTENER = "hibernate.session.events.auto";
    
201.  String PROCEDURE_NULL_PARAM_PASSING = "hibernate.proc.param_null_passing";
    
202.  String CREATE_EMPTY_COMPOSITES_ENABLED = "hibernate.create_empty_composites.enabled";
    
203.  String ALLOW_JTA_TRANSACTION_ACCESS = "hibernate.jta.allowTransactionAccess";
    
204.  String ALLOW_UPDATE_OUTSIDE_TRANSACTION = "hibernate.allow_update_outside_transaction";
    
205.  String COLLECTION_JOIN_SUBQUERY = "hibernate.collection_join_subquery";
    
206.  String ALLOW_REFRESH_DETACHED_ENTITY = "hibernate.allow_refresh_detached_entity";
    
207.  String MERGE_ENTITY_COPY_OBSERVER = "hibernate.event.merge.entity_copy_observer";
    
208.  String USE_LEGACY_LIMIT_HANDLERS = "hibernate.legacy_limit_handler";
    
209.  String VALIDATE_QUERY_PARAMETERS = "hibernate.query.validate_parameters";
    
210.  String CRITERIA_LITERAL_HANDLING_MODE = "hibernate.criteria.literal_handling_mode";
    
211.  String PREFER_GENERATOR_NAME_AS_DEFAULT_SEQUENCE_NAME = "hibernate.model.generator_name_as_sequence_name";
    
212.  String JPA_TRANSACTION_COMPLIANCE = "hibernate.jpa.compliance.transaction";
    
213.  String JPA_QUERY_COMPLIANCE = "hibernate.jpa.compliance.query";
    
214.  String JPA_LIST_COMPLIANCE = "hibernate.jpa.compliance.list";
    
215.  String JPA_CLOSED_COMPLIANCE = "hibernate.jpa.compliance.closed";
    
216.  String JPA_PROXY_COMPLIANCE = "hibernate.jpa.compliance.proxy";
    
217.  String JPA_CACHING_COMPLIANCE = "hibernate.jpa.compliance.caching";
    
218.  String JPA_ID_GENERATOR_GLOBAL_SCOPE_COMPLIANCE = "hibernate.jpa.compliance.global_id_generators";
    
219.  String TABLE_GENERATOR_STORE_LAST_USED = "hibernate.id.generator.stored_last_used";
    
220.  String FAIL_ON_PAGINATION_OVER_COLLECTION_FETCH = "hibernate.query.fail_on_pagination_over_collection_fetch";
    
221.  String IMMUTABLE_ENTITY_UPDATE_QUERY_HANDLING_MODE = "hibernate.query.immutable_entity_update_query_handling_mode";
    
222.  String IN_CLAUSE_PARAMETER_PADDING = "hibernate.query.in_clause_parameter_padding";
    
223.  String QUERY_STATISTICS_MAX_SIZE = "hibernate.statistics.query_max_size";
    
224.  String SEQUENCE_INCREMENT_SIZE_MISMATCH_STRATEGY = "hibernate.id.sequence.increment_size_mismatch_strategy";
    
225.  String OMIT_JOIN_OF_SUPERCLASS_TABLES = "hibernate.query.omit_join_of_superclass_tables";
    

`
