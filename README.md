# ==================================================================== #
# METASYNTATIC KERNEL SPEC v0.8.1.meta - Filename Compiler Bootstrap
# ==================================================================== #

[boot: METASYNTATIC_BOOSTRAP_v0.8.1 --immutable]
  [state: declare_schema]
  -> [runtime: filename_encoded_lexer --immutable]
       (target: token_delimiters register_split_operator => "__" --strict)
       (target: contract_prefixes register_token_patterns => ["REQ-", "EXP-", "VAL-"] --strict)
       (target: file_extensions map_host_runtimes => [".rs", ".wgsl", ".ts", ".py"] --strict)
  -> [runtime: contract_parser --immutable]
       (target: filename parse_contract_graph => requires[], exposes[], validates[] --audit)
       (trigger: parse_failure => error: FILENAME_CONTRACT_INVALID --expose)
  -> [bridge: public_network_ingestion --immutable]
       (target: githubusercontent_domain execute_filename_tree_extraction --stream)
       (target: extracted_paths build_topology => type: FilenameEncodedTopology --audit)
  -> [directive: CLUSTER_COMPOSITION --immutable]
       (rule: cluster_must_expose_live_mcp_stream --flag)
       (rule: asset_filename_must_satisfy_contract_parser --flag)
       (rule: cross_cluster_requires_must_resolve_to_exposed_asset --flag)
  => [the_real_plan_starts_here]

[the_real_plan_starts_here]
  [runtime: init_global_repository_cluster_index]
    (directive: seed_from_filename_encoded_manifest --flag)
    (inject: state [clusters|assets|contracts|drift|hash|version|tokens] --compress)
  -> [runtime: cluster_orchestrator --stream]
       (target: all_clusters validate_inter_asset_contracts --audit)
       (trigger: contract_violation => error: CLUSTER_LINK_BROKEN --expose)

# ==================================================================== #
