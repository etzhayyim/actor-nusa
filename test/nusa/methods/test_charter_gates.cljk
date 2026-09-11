(ns nusa.methods.test-charter-gates
  "nusa 幣 — constitutional-gate conformance tests (manifest + local lexicons).

  Substrate-native Clojure (clj + datomic first tier). nusa datafies ritual/industrial HEMP
  heritage + low-THC cultivation revival — recreational THC is structurally EXCLUDED. It reads
  the first-tier `contracts/lexicons/*.edn` and canonical manifest via clojure.edn. Its 10 gates are
  declared in the manifest and encoded as enum/const across the 5 lexicons. This suite pins them
  so a future R-phase cell wave cannot silently drift them:

    G1/G2 thc-class is {fiber, low-thc} ONLY — `:psychoactive` is unrepresentable (the headline
          gate); purpose is fibre/ritual only, never recreational/consumption
    G5  no-server-key — licence is member-principal, server-held-key const false, member-signed
    G4  member-principal / no-fiat-inflow — fundingSource is member-okaimono only
    G7  sourcing-honesty — every record carries a sourcing ∈ {representative, synthesized, verified};
        fibre provenance is screened
    G8  outward-gated — the cultivation licence plan is outwardGated const true
    G3  non-adjudicating — heritage record carries a nonAdjudicatingNotice

  Reads canonical EDN only. It weakens no gate; it asserts
  them. G5 IS the substrate no-server-key invariant for this actor; Murakumo-only is manifest G6."
  (:require [clojure.test :refer [deftest is run-tests]]
            [clojure.edn :as edn]))

#?(:clj
   (do
     (def ^:private here (.getParentFile (java.io.File. ^String *file*)))
     (def ^:private actor-dir (nth (iterate #(.getParentFile %) here) 3))
     (def ^:private lexdir (java.io.File. actor-dir "contracts/lexicons"))
     (defn- lex [name]
       (edn/read-string (slurp (java.io.File. lexdir (str name ".edn")))))
     (defn- manifest []
       (edn/read-string (slurp (java.io.File. actor-dir "manifest.edn"))))))

(defn- record-node [doc] (get-in doc [:defs :main :record]))
(defn- required-of [doc] (set (:required (record-node doc))))
(defn- enum-of [doc field] (set (get-in (record-node doc) [:properties field :enum])))
(defn- const-of [doc field] (get-in (record-node doc) [:properties field :const]))

;; ── 10 gates + non-goals declared ──
(deftest gates-and-nongoals-declared
  (let [gm (:actor/gates (manifest))
        gn (->> gm (map :gate/id) (keep #(second (re-matches #"G(\d+)" %)))
                (map #(Integer/parseInt %)) set)]
    (is (= (set (range 1 11)) gn) "manifest must declare G1–G10")
    (is (seq (:actor/non-goals (manifest))) "manifest must declare non-goals")))

;; ── G1/G2 — thc-class is {fiber, low-thc} ONLY; :psychoactive unrepresentable ──
(deftest g1-g2-thc-class-no-psychoactive
  (doseq [n ["hempCultivar" "fiberProvenance"]]
    (let [e (enum-of (lex n) :thcClass)]
      (is (= #{"fiber" "low-thc"} e)
          (str "G1/G2: " n ".thcClass must be {fiber, low-thc} (no psychoactive), got " e))
      (is (not (contains? e "psychoactive"))
          (str "G1/G2: " n " must NOT make psychoactive representable")))))

;; ── G2 — purpose is fibre/ritual only, never recreational ──
(deftest g2-purpose-non-recreational
  (is (= #{"ritual-fiber" "industrial-fiber"} (enum-of (lex "cultivationLicensePlan") :purpose))
      "G2: cultivation purpose must be ritual/industrial fibre only")
  (is (= "purification" (const-of (lex "ritualArtifact") :purpose))
      "G2: a ritual artifact's purpose is purification (const)"))

;; ── G5 — no-server-key: member-principal, server-held-key false, member-signed ──
(deftest g5-no-server-key
  (let [p (lex "cultivationLicensePlan")]
    (is (contains? (required-of p) "serverHeldKey") "G5: serverHeldKey must be a required field")
    (is (= false (const-of p :serverHeldKey)) "G5: serverHeldKey const false")
    (is (= "member" (const-of p :licenseePrincipal)) "G5: licenseePrincipal const member")
    (is (= #{"member"} (enum-of p :signedBy)) "G5: signedBy must be member only")
    (is (= true (const-of p :outwardGated)) "G8: outwardGated const true")))

;; ── G4 — member-principal / no-fiat-inflow ──
(deftest g4-member-principal-funding
  (is (= #{"member-okaimono"} (enum-of (lex "cultivationLicensePlan") :fundingSource))
      "G4: fundingSource must be member-okaimono only (no religious-corp fiat inflow)"))

;; ── G7 — sourcing-honesty on every record; fibre provenance screened ──
(deftest g7-sourcing-honesty
  (doseq [n ["hempCultivar" "fiberProvenance" "ritualArtifact" "heritageRecord"]]
    (is (= #{"representative" "synthesized" "verified"} (enum-of (lex n) :sourcing))
        (str "G7: " n ".sourcing must be the honest 3-value set")))
  (is (= true (const-of (lex "fiberProvenance") :screened))
      "G7: fibre provenance is THC-class screened (const true)"))

;; ── G3 — non-adjudicating heritage record ──
(deftest g3-non-adjudicating
  (is (= true (const-of (lex "heritageRecord") :nonAdjudicatingNotice))
      "G3: heritage record carries a nonAdjudicatingNotice (const true)"))

#?(:clj
   (defn -main [& _]
     (let [r (run-tests 'nusa.methods.test-charter-gates)]
       (System/exit (if (zero? (+ (:fail r) (:error r))) 0 1)))))
