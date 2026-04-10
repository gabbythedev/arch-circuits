# Architecture Circuits - MVP Implementation Plan

**Status:** Ready for development  
**Target Timeline:** 8-12 weeks  
**Phase 1 (Weeks 1-4):** Levels 1-3 MVP with pedagogical scaffolding  
**Phase 2 (Weeks 5-8):** Levels 4-7 expansion  
**Phase 3 (Weeks 9-12):** Internal validation & iteration  

---

## Project Setup & Architecture

### Tech Stack
- **Frontend:** React 18 + TypeScript
- **Canvas Engine:** React Flow (for graph/component visualization)
- **Simulation:** State machine pattern (XState recommended for complexity management)
- **Storage:** LocalStorage for v1 (session progress, user choices)
- **Analytics:** Basic event tracking (level completion, time-on-task, shares)
- **Build:** Vite (fast dev server, optimized build)
- **Testing:** Vitest + React Testing Library

### Project Structure
```
arch-circuits/
├── public/
│   └── assets/
│       ├── components/ (SVG icons for DB, cache, API, etc.)
│       ├── backgrounds/
│       └── audio/ (optional: success/failure sounds)
├── src/
│   ├── App.tsx (main entry)
│   ├── pages/
│   │   ├── HomePage.tsx
│   │   ├── ModuleSelector.tsx
│   │   └── LevelPlayer.tsx
│   ├── components/
│   │   ├── Canvas/ (React Flow wrapper)
│   │   │   ├── ArchitectureCanvas.tsx
│   │   │   ├── ComponentNode.tsx
│   │   │   └── WireEdge.tsx
│   │   ├── Sidebar/
│   │   │   ├── ComponentConfig.tsx
│   │   │   └── PropertyPanel.tsx
│   │   ├── Simulation/
│   │   │   ├── SimulationViewer.tsx
│   │   │   ├── RequestTracer.tsx
│   │   │   └── FailureExplainer.tsx
│   │   ├── Pedagogical/
│   │   │   ├── LevelIntro.tsx
│   │   │   ├── FailurePrompt.tsx
│   │   │   ├── ReflectionPrompt.tsx
│   │   │   ├── PostLevelAssessment.tsx
│   │   │   └── ExplanationBox.tsx
│   │   └── Common/
│   │       ├── ProgressBar.tsx
│   │       ├── Button.tsx
│   │       └── Modal.tsx
│   ├── hooks/
│   │   ├── useSimulation.ts
│   │   ├── useComponentState.ts
│   │   └── usePersistence.ts
│   ├── services/
│   │   ├── simulationEngine/ (state machine for system behavior)
│   │   │   ├── BikeStoreSimulator.ts
│   │   │   ├── componentBehaviors/
│   │   │   └── requestSimulation.ts
│   │   ├── analytics.ts (track time, completion, events)
│   │   ├── pedagogyEngine.ts (manage difficulty, prompts, feedback)
│   │   └── storage.ts (LocalStorage abstraction)
│   ├── types/
│   │   ├── component.ts (Component interface)
│   │   ├── scenario.ts (Level/Module structure)
│   │   ├── simulation.ts (Request, failure, response types)
│   │   └── pedagogy.ts (Assessment, feedback, reflection)
│   ├── levels/ (Scenario definitions)
│   │   ├── bikeStore/
│   │   │   ├── level1.ts (Browse catalog)
│   │   │   ├── level2.ts (Database queries)
│   │   │   ├── level3.ts (Caching)
│   │   │   ├── level4.ts (Payment)
│   │   │   ├── level5.ts (Queues)
│   │   │   ├── level6.ts (Notifications)
│   │   │   └── level7.ts (Retries)
│   │   └── scenarios/ (Template for future modules)
│   ├── styles/
│   │   ├── globals.css
│   │   ├── canvas.css
│   │   └── animations.css
│   └── utils/
│       ├── componentFactory.ts
│       └── validators.ts
├── tests/
│   ├── simulation.test.ts
│   ├── pedagogy.test.ts
│   └── components/
├── .env.example
├── tsconfig.json
├── vite.config.ts
├── package.json
└── README.md
```

---

## Phase 1: Levels 1-3 MVP (Weeks 1-4)

**Goal:** Build and validate the pedagogical learning loop with Levels 1-3. Prove that the interaction model teaches architecture effectively.

### Week 1: Foundation & Level 1

**Tasks:**
1. **Project Setup**
   - Initialize Vite + React + TypeScript
   - Install dependencies: React Flow, XState, testing libraries
   - Configure TypeScript, ESLint, Prettier
   - Set up git workflow and branch structure

2. **Level 1: Browse Catalog (Simple baseline)**
   - **Components:** Browser, Cloud Storage
   - **Interaction:** User drags Browser to canvas, connects to Cloud Storage, views a simple catalog GET request
   - **No failures:** Level 1 just proves the interaction works
   - **Success:** Request completes, user sees data displayed
   - **Learning Goal:** Understand component connection and data flow visualization

**Deliverables:**
- React Flow canvas working with draggable components
- Basic component configuration sidebar
- Simple simulation engine for GET request (no failures)
- LocalStorage persistence of canvas state
- Time-on-task tracking

**Success Criteria:**
- Level 1 loads in < 2 seconds
- Users can drag components and connect them
- Simulation visualization is clear and intuitive
- Time-on-level tracked and persisted

---

### Week 2: Level 1 Refinement + Level 2 Scaffolding

**Tasks:**
1. **Level 1 Polish**
   - Add visual feedback on successful request (animation, color change)
   - Implement level intro ("Welcome to the Bike Store...here's what you're building")
   - Add basic help/tooltips for first-time users
   - Test on 2-3 external users (bootcamp partners if possible)

2. **Level 2: Database Queries (Introduce data persistence)**
   - **Components:** Browser, API Server, Cloud SQL
   - **Interaction:** User adds database, configures it, sees query latency
   - **Failures:** None yet; focus on understanding persistence layer
   - **Learning Goal:** Understand that data needs to be stored; queries take time

**Pedagogical Design (Critical):**
- Level intro explains WHY databases exist ("Orders need to be stored")
- Component config shows "Query Time: 50ms" so users see latency concept
- Simulation shows request → DB query → response (with timing)
- Post-level reflection: "Why did we add a database? What would break without it?"

**Deliverables:**
- Level 2 fully playable
- Post-level reflection prompts implemented
- Time-on-task tracking verified
- User feedback incorporated from Level 1 testing

---

### Week 3: Level 3 + Assessment Design

**Tasks:**
1. **Level 3: Caching (Introduce performance trade-offs - THE core teaching moment)**
   - **Components:** Browser, API Server, Cache, Cloud SQL
   - **Guided Failure:** "Try serving 10x normal traffic without cache"
   - **Observation:** Requests back up, latency spikes, some fail
   - **Explanation:** "Without cache, every request hits the database. During traffic spikes, the DB can't keep up."
   - **Learning Goal:** Understand cache trade-offs (speed vs. staleness)

**Pedagogical Scaffolding (Critical):**
- Level intro: "The bike store is having a sale. Orders flood in. What happens?"
- Guided failure prompt: "What happens if you remove the cache?"
- When cache is removed + high traffic triggered:
  - Visual: Requests slow down, some fail (red indicators)
  - Explanation: "Your cache expired. The database is overwhelmed."
  - Follow-up: "What could you do differently?"
- Post-level assessment: "Explain the trade-off between cache freshness and performance"

2. **Assessment Design**
   - Post-level concept quiz (3 questions per level, graded)
   - Reflection prompts: "Explain in your own words why we added [component]"
   - Transfer question: "If traffic doubled again, what would break now?"
   - Store assessment results for analytics

**Deliverables:**
- Level 3 with guided failure scenarios fully implemented
- Post-level assessment framework operational
- Explanation messaging tied to failures
- Learning outcome data collected

---

### Week 4: Validation & Iteration

**Tasks:**
1. **Internal EPC Team Testing**
   - Deploy Levels 1-3 to 5-10 junior developers (EPC team)
   - Measure:
     - Time-on-level per user
     - Completion rates
     - Assessment accuracy (pre vs. post)
     - Reflection quality (can they explain trade-offs?)
     - Engagement (do they return to next level?)

2. **Data Analysis & Iteration**
   - Review analytics dashboard
   - Identify where users get stuck (e.g., Level 2 taking too long = cognitive overload)
   - Refine explanations based on assessment results
   - Adjust difficulty (e.g., traffic scaling, timeout thresholds)

3. **Success Validation**
   - ✅ 70%+ users spend 15-40 min on Level 1
   - ✅ 60%+ complete Levels 1-3
   - ✅ 70%+ pass post-level assessments
   - ✅ Users articulate cache trade-offs in reflection prompts

**If validation succeeds:** Move to Phase 2  
**If validation struggles:** Iterate on pedagogy (explanations, difficulty, feedback)

---

## Phase 2: Levels 4-7 Expansion (Weeks 5-8)

**Goal:** Complete the full e-commerce scenario while maintaining pedagogical rigor.

### Level 4: Payment Processing
- **Components:** Add Payment Gateway
- **Learning Goal:** Understand transaction complexity, error handling
- **Guided Failure:** "What happens if payment fails mid-transaction?"
- **Assessment:** Explain idempotency, transaction isolation

### Level 5: Order Queue
- **Components:** Add Queue, Worker Service
- **Learning Goal:** Understand async processing, throughput improvement
- **Guided Failure:** "What happens without the queue under high load?"
- **Assessment:** Explain when to use queues, trade-offs (latency vs. throughput)

### Level 6: Notifications
- **Components:** Add Email/Notification Service
- **Learning Goal:** Understand eventual consistency, notification patterns
- **Guided Failure:** "What if notifications fail but orders process?"
- **Assessment:** Explain notification reliability strategies

### Level 7: Failure Recovery
- **Components:** Add Retry Logic, Circuit Breaker (optional for v1)
- **Learning Goal:** Understand resilience, graceful degradation
- **Guided Failure:** "Turn off payment service; what happens?"
- **Assessment:** Explain retry strategies, failure modes

---

## Phase 3: Validation & Bootcamp Pilot (Weeks 9-12)

**Tasks:**
1. **Bootcamp Integration**
   - Deploy to 1-2 bootcamp partners
   - 50+ bootcamp students complete Levels 1-7
   - Measure learning outcomes: pre-test → post-test architecture knowledge
   - Gather qualitative feedback: "What helped you understand architecture?"

2. **Case Study Development**
   - Quantify: "XX% faster onboarding with Arch-Circuits vs. traditional"
   - Document: Architecture concepts junior devs learned
   - Testimonials from bootcamp instructors and students

3. **Analytics & Success Metrics**
   - ✅ 70%+ completion of Level 1
   - ✅ 60%+ completion of Level 3
   - ✅ 50%+ completion of Level 7
   - ✅ 70%+ post-level assessment accuracy
   - ✅ Viral coefficient: 25%+ peer recommendations
   - ✅ Return rate: 60%+ return within 7 days

---

## Critical Implementation Details

### Simulation Engine (Core Technology)
The simulation engine is the "magic" that makes learning possible. It must:
1. Deterministically simulate system behavior (requests flow through components predictably)
2. Provide rich failure modes (timeouts, errors, bottlenecks, cascading failures)
3. Support difficulty scaling (traffic increase, resource constraints, failure injection)
4. Generate explanations tied to failures (not just "error 500", but "Database overloaded")

**Recommendation:** Use XState for state machine clarity. Example:

```typescript
const bikeStoreMachine = createMachine({
  initial: 'idle',
  states: {
    idle: { on: { TRIGGER_TRAFFIC: 'processing' } },
    processing: {
      on: {
        REQUEST_SUCCESS: { target: 'success' },
        CACHE_MISS: { target: 'slowPath' },
        DB_OVERLOAD: { target: 'failure' }
      }
    },
    failure: { on: { RESOLVE: 'idle' } }
  }
});
```

### Pedagogical Engine (Core to Learning)
Separate logic that manages:
1. **Difficulty progression:** When to introduce new failure scenarios
2. **Feedback generation:** When a failure occurs, generate user-friendly explanation
3. **Reflection prompts:** Questions that force metacognition
4. **Assessment:** Measure whether learning happened

Example:
```typescript
const pedagogyEngine = {
  getExplanation(failureType: 'cache_miss' | 'db_overload') {
    if (failureType === 'cache_miss') {
      return "Your cache expired. The database is being hit directly, causing requests to slow down.";
    }
  },
  getReflectionPrompt(level: number) {
    // Level-specific reflection to check mental model
  },
  assessLearning(userResponses: Assessment[]) {
    // Did they actually learn the concept?
  }
};
```

### Analytics Tracking (Success Measurement)
Track every critical event:
- Level started / completed (with timestamp)
- Time spent per level
- Assessment results (pre/post)
- User shares / peer recommendations
- Return rate (did they come back?)

LocalStorage is sufficient for v1, but structure data so it can be easily sent to a backend later:

```typescript
const events = [
  { type: 'level_started', level: 1, timestamp: ... },
  { type: 'simulation_triggered', level: 1, components: [...] },
  { type: 'failure_observed', failure_type: 'cache_miss', level: 3 },
  { type: 'assessment_completed', level: 3, score: 0.85 },
  { type: 'shared', shared_with: 'peer', level: 3 }
];
```

---

## Success Criteria & Go/No-Go Decision Points

### End of Phase 1 (Week 4)
- ✅ Levels 1-3 fully playable
- ✅ Pedagogical loop validated (reflection, assessment, failure explanations working)
- ✅ 70%+ EPC team completes Levels 1-3
- ✅ Post-level assessments show 70%+ accuracy
- ✅ Users articulate mental model shifts

**If YES:** Proceed to Phase 2  
**If NO:** Iterate on pedagogy (explanations, difficulty, feedback) before expanding

### End of Phase 2 (Week 8)
- ✅ Levels 4-7 fully playable
- ✅ Learning outcomes measured across all 7 levels
- ✅ Difficulty progression balanced (no cliffs)
- ✅ 50%+ complete Level 7

**If YES:** Proceed to Phase 3 (bootcamp validation)  
**If NO:** Debug and iterate before bootcamp rollout

### End of Phase 3 (Week 12)
- ✅ Bootcamp students show significant learning gains (pre/post)
- ✅ 25%+ peer recommendation rate (viral signal)
- ✅ Case study documented and ready for marketing
- ✅ Architecture ready for scale (organizational features, additional modules)

**If YES:** Ready for product launch roadmap  
**If NO:** Gather feedback, refine, plan v1.1

---

## Development Workflow

**Branch strategy:**
- `main` — Production-ready code
- `develop` — Integration branch for features
- `feature/level-X` — Individual level development
- `feature/pedagogy` — Pedagogy engine improvements

**Definition of Done (per task):**
- Code written with tests (components, simulation logic)
- Pedagogical design reviewed (explanations, assessments, prompts)
- Analytics tracking verified
- Works on laptop + deployed to staging
- User testing completed (internal or bootcamp)

**Daily standup focus:**
- Pedagogical rigor (is learning actually happening?)
- Time-on-task metrics (are users in Goldilocks zone?)
- User feedback (where are they getting stuck?)

---

## Tools & Resources

**Required:**
- GitHub (repo management) ✅
- Node.js 18+
- VS Code or IDE of choice
- React Flow documentation
- XState documentation

**Recommended:**
- Figma (mockup Level UI before coding)
- Google Sheets (track analytics during testing)
- Loom (record user testing sessions for analysis)

---

## Next Steps

1. **Immediately (This week):**
   - Set up project structure (Vite + React + TypeScript)
   - Initialize React Flow with example canvas
   - Create Level 1 skeleton (Browser + Cloud Storage components)
   - Set up basic state machine for request simulation

2. **Week 1 checkpoint:**
   - Level 1 interaction working (drag, connect, simulate)
   - LocalStorage persistence functional
   - Time tracking active

3. **Ongoing:**
   - Daily reviews of pedagogical design decisions
   - Weekly check of time-on-task metrics
   - Weekly user testing (even if informal)

---

**Ready to kick off Week 1? I can help with project setup and the first Level 1 implementation sprint.**
