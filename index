import React, { useMemo, useState } from "react";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Textarea } from "@/components/ui/textarea";
import { Input } from "@/components/ui/input";
import { Badge } from "@/components/ui/badge";
import { Shuffle, Users, Download, RefreshCw } from "lucide-react";

const POSITION_POOL = [
  "Public colleges should offer tuition-free education for students from low- and middle-income families.",
  "Cities should expand public transit even when it requires higher local taxes.",
  "Social media companies should remove false or misleading political content more aggressively.",
  "The federal minimum wage should be raised nationwide.",
  "Student loan forgiveness should be expanded for borrowers with the greatest financial need.",
  "Local governments should invest more in affordable housing, even if some neighborhoods change.",
  "Employers should be required to provide paid family leave.",
  "States should limit the use of facial recognition technology in public spaces.",
  "Schools and colleges should restrict the use of generative AI in graded coursework.",
  "Communities should prioritize climate resilience projects over some other infrastructure spending.",
  "The United States should reduce cash bail for nonviolent offenses.",
  "Hospitals should be required to publish clearer and more accessible pricing information.",
  "States should make Election Day a state holiday to increase voter participation.",
  "Local governments should place stronger limits on short-term rentals to protect housing access.",
  "Colleges should require a civic dialogue course focused on disagreement across differences.",
  "The government should regulate data collection by apps more strictly.",
  "Public schools should provide free breakfast and lunch to every student.",
  "Communities should create more supervised safe-use strategies to reduce overdose deaths.",
  "States should increase funding for mental health care even if it requires budget tradeoffs.",
  "Large employers should be more transparent about pay ranges in job postings.",
  "Cities should reduce car traffic downtown to create more pedestrian-friendly spaces.",
  "The federal government should invest more in job training than in four-year degree expansion.",
  "Colleges should guarantee basic-needs support for students facing food or housing insecurity.",
  "News organizations should label AI-generated content clearly and prominently.",
  "Communities should expand broadband access as a public necessity, even in high-cost rural areas.",
  "The government should place stricter limits on prescription drug prices.",
  "States should restore voting rights automatically after prison sentences are completed.",
  "Universities should reduce legacy admissions in favor of broader access policies."
];

const AUDIENCE_PAIRS = [
  {
    title: "Economic Security vs Tax Burden",
    audiences: [
      "A low-income parent working multiple jobs and worrying about housing, food, and childcare costs.",
      "A high-income taxpayer who values limited government spending and worries about inefficiency."
    ]
  },
  {
    title: "Rural Traditionalist vs Urban Progressive",
    audiences: [
      "A politically conservative rural resident who values tradition, local control, and skepticism of rapid change.",
      "A politically progressive urban resident who values equity, reform, and collective responsibility."
    ]
  },
  {
    title: "Small Business vs Labor Advocate",
    audiences: [
      "A small business owner concerned about regulations, payroll pressure, and staying competitive.",
      "A worker-rights advocate focused on fair pay, safety, and power imbalances in the workplace."
    ]
  },
  {
    title: "First-Generation Student vs Affluent Professional",
    audiences: [
      "A first-generation college student balancing classes, work, and family obligations with limited financial cushion.",
      "An affluent professional who values merit, efficiency, and personal responsibility."
    ]
  },
  {
    title: "Government Skeptic vs Public Systems Supporter",
    audiences: [
      "A person who distrusts government institutions and worries about overreach.",
      "A person who believes public institutions are necessary for fairness and social stability."
    ]
  },
  {
    title: "Recent Layoff vs Tech Professional",
    audiences: [
      "A recently laid-off worker who feels economically vulnerable and uncertain about the future.",
      "A financially stable tech professional who prioritizes innovation and long-term growth."
    ]
  },
  {
    title: "Community Stability vs Social Change",
    audiences: [
      "A homeowner focused on neighborhood stability, predictability, and property values.",
      "A community organizer focused on access, inclusion, and correcting systemic inequities."
    ]
  },
  {
    title: "Immigrant Family vs Native-Born Taxpayer",
    audiences: [
      "A recent immigrant family navigating unfamiliar systems and seeking opportunity and dignity.",
      "A native-born taxpayer concerned about fairness, resource allocation, and social cohesion."
    ]
  }
];

function shuffle(array) {
  const arr = [...array];
  for (let i = arr.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

function buildGroups(names, minSize, maxSize) {
  const shuffled = shuffle(names.filter(Boolean));
  const total = shuffled.length;
  if (total === 0) return [];

  let groupCount = Math.ceil(total / maxSize);
  while (groupCount > 1 && total / groupCount < minSize) {
    groupCount -= 1;
  }

  const baseSize = Math.floor(total / groupCount);
  let remainder = total % groupCount;
  const sizes = Array.from({ length: groupCount }, () => baseSize).map((size) => {
    if (remainder > 0) {
      remainder -= 1;
      return size + 1;
    }
    return size;
  });

  const groups = [];
  let index = 0;
  sizes.forEach((size, idx) => {
    groups.push({
      groupNumber: idx + 1,
      students: shuffled.slice(index, index + size),
    });
    index += size;
  });

  return groups;
}

function assignPositions(names) {
  const pool = shuffle(POSITION_POOL);
  return names.map((name, idx) => ({
    name,
    position: pool[idx % pool.length],
  }));
}

function assignAudiencePairs(groups) {
  const shuffledPairs = shuffle(AUDIENCE_PAIRS);
  return groups.map((group, idx) => ({
    ...group,
    pair: shuffledPairs[idx % shuffledPairs.length],
  }));
}

export default function EmpathyEthicsSpeechTool() {
  const [studentInput, setStudentInput] = useState("Alex\nJordan\nTaylor\nSam\nCasey\nMorgan\nRiley\nAvery\nParker\nQuinn\nJamie\nDrew");
  const [groupSize, setGroupSize] = useState("4");
  const [seed, setSeed] = useState(1);

  const studentNames = useMemo(
    () => studentInput.split("\n").map((s) => s.trim()).filter(Boolean),
    [studentInput]
  );

  const generated = useMemo(() => {
    // seed changes force recomputation with new randomization
    seed;
    const groups = buildGroups(studentNames, 3, Number(groupSize) || 4);
    const withAudiences = assignAudiencePairs(groups);
    const positions = assignPositions(studentNames);
    const positionMap = new Map(positions.map((p) => [p.name, p.position]));

    return withAudiences.map((group) => ({
      ...group,
      students: group.students.map((name) => ({
        name,
        position: positionMap.get(name),
      })),
    }));
  }, [studentNames, groupSize, seed]);

  const exportText = () => {
    const lines = [];
    generated.forEach((group) => {
      lines.push(`GROUP ${group.groupNumber}`);
      lines.push(`Audience Pair Theme: ${group.pair.title}`);
      lines.push(`Audience A: ${group.pair.audiences[0]}`);
      lines.push(`Audience B: ${group.pair.audiences[1]}`);
      lines.push("");
      group.students.forEach((student) => {
        lines.push(`${student.name}: ${student.position}`);
      });
      lines.push("\n----------------------------------------\n");
    });

    const blob = new Blob([lines.join("\n")], { type: "text/plain;charset=utf-8" });
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = "speech-empathy-group-assignments.txt";
    a.click();
    URL.revokeObjectURL(url);
  };

  return (
    <div className="min-h-screen bg-background p-6 md:p-10">
      <div className="mx-auto max-w-7xl space-y-6">
        <div className="space-y-2">
          <h1 className="text-3xl font-semibold tracking-tight">Empathy + Ethical Speaking Group Generator</h1>
          <p className="max-w-3xl text-sm text-muted-foreground">
            Generate a different clear position statement for each student, then place students into small groups of 3–4 who share the same contrasting audiences for the harm-awareness adaptation activity.
          </p>
        </div>

        <div className="grid gap-6 lg:grid-cols-[360px_1fr]">
          <Card className="rounded-2xl shadow-sm">
            <CardHeader>
              <CardTitle className="text-lg">Class Setup</CardTitle>
            </CardHeader>
            <CardContent className="space-y-4">
              <div className="space-y-2">
                <label className="text-sm font-medium">Student names (one per line)</label>
                <Textarea
                  value={studentInput}
                  onChange={(e) => setStudentInput(e.target.value)}
                  className="min-h-[280px]"
                />
              </div>

              <div className="space-y-2">
                <label className="text-sm font-medium">Preferred maximum group size</label>
                <Input
                  value={groupSize}
                  onChange={(e) => setGroupSize(e.target.value)}
                  type="number"
                  min="3"
                  max="4"
                />
                <p className="text-xs text-muted-foreground">Use 4 for groups of 3–4. The tool balances group sizes automatically.</p>
              </div>

              <div className="flex flex-wrap gap-2">
                <Button onClick={() => setSeed((s) => s + 1)} className="rounded-2xl">
                  <Shuffle className="mr-2 h-4 w-4" />
                  Regenerate
                </Button>
                <Button variant="outline" onClick={exportText} className="rounded-2xl">
                  <Download className="mr-2 h-4 w-4" />
                  Export TXT
                </Button>
              </div>

              <div className="rounded-2xl border p-3 text-sm text-muted-foreground">
                <p className="font-medium text-foreground">How to use</p>
                <ol className="mt-2 list-decimal space-y-1 pl-5">
                  <li>Paste your class roster.</li>
                  <li>Click <span className="font-medium">Regenerate</span> until you like the mix.</li>
                  <li>Each student gets an individual position statement.</li>
                  <li>Students in the same group share the same two contrasting audiences for the next phase of the activity.</li>
                </ol>
              </div>
            </CardContent>
          </Card>

          <div className="space-y-4">
            <div className="flex items-center justify-between rounded-2xl border bg-card p-4 shadow-sm">
              <div>
                <p className="text-sm text-muted-foreground">Students</p>
                <p className="text-2xl font-semibold">{studentNames.length}</p>
              </div>
              <div>
                <p className="text-sm text-muted-foreground">Groups</p>
                <p className="text-2xl font-semibold">{generated.length}</p>
              </div>
              <div>
                <p className="text-sm text-muted-foreground">Group target</p>
                <p className="text-2xl font-semibold">3–{groupSize || 4}</p>
              </div>
            </div>

            <div className="grid gap-4 xl:grid-cols-2">
              {generated.map((group) => (
                <Card key={group.groupNumber} className="rounded-2xl shadow-sm">
                  <CardHeader className="space-y-3">
                    <div className="flex items-center justify-between">
                      <CardTitle className="text-lg">Group {group.groupNumber}</CardTitle>
                      <Badge variant="secondary" className="rounded-xl">
                        <Users className="mr-1 h-3.5 w-3.5" />
                        {group.students.length} students
                      </Badge>
                    </div>
                    <div className="rounded-2xl border p-3">
                      <p className="text-sm font-medium">Shared audience contrast</p>
                      <p className="mt-1 text-sm text-muted-foreground">{group.pair.title}</p>
                      <div className="mt-3 space-y-2 text-sm">
                        <div>
                          <span className="font-medium">Audience A:</span> {group.pair.audiences[0]}
                        </div>
                        <div>
                          <span className="font-medium">Audience B:</span> {group.pair.audiences[1]}
                        </div>
                      </div>
                    </div>
                  </CardHeader>
                  <CardContent>
                    <div className="space-y-3">
                      {group.students.map((student) => (
                        <div key={student.name} className="rounded-2xl border p-3">
                          <div className="flex items-center justify-between gap-3">
                            <p className="font-medium">{student.name}</p>
                            <Button variant="ghost" size="sm" onClick={() => setSeed((s) => s + 1)}>
                              <RefreshCw className="h-4 w-4" />
                            </Button>
                          </div>
                          <p className="mt-2 text-sm text-muted-foreground">{student.position}</p>
                        </div>
                      ))}
                    </div>
                  </CardContent>
                </Card>
              ))}
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
